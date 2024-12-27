**Time-of-Check to Time-of-Use (TOCTOU) flaw**

When user submits their coupon code, in actual code of web app, a check is performed that the coupon is valid and hasn’t been used before. The discount is applied and only then the coupon code will be updated to indicate that it has already been used. 

![12Ss01](https://github.com/user-attachments/assets/61ea3550-250d-4c3a-bdce-266bf68c1e4e)

In above example, between the check if the coupon is valid and the update of the coupon being used, there are a couple of milliseconds where the coupon is applied. While it might seem small, if a threat actor can send two requests so close together in time, it might happen that before the coupon is updated in request 1, it has already been checked in request 2, meaning that both requests will apply the coupon

**Intercepting the Request**

![12SS02](https://github.com/user-attachments/assets/0dfa8c71-031f-4067-a775-0a36b08ce745)

Two functionalities:

1. **logout** - does not involve simultaneous tasks
2. **fund transfer** - includes deducting funds from the account and adding them to other account
    - could be an opportunity for an attack (as a pentester, test/exploit the vulnerability)

**Verify fund transfer functionality**

![12SS03](https://github.com/user-attachments/assets/1387e5b1-b207-4fd3-bec5-ddbd42e8a60f)

![12SS04](https://github.com/user-attachments/assets/609cc1dd-215e-4902-964b-6349ddeacb63)

/transfer endpoint accepts a POST request with parameters *account_number* and *amount*. 

Burp suite *Repeater* tool allows sending of multiple HTTP requests

Use this feature to duplicate HTTP POST request and send it multiple times to exploit race condition vulnerability.

![12SS05](https://github.com/user-attachments/assets/b3688c20-2c75-4733-870c-0d631a417847)

POST request that needs to be triggered multiple times. 

Here, can change *account_number* and *amount value*

![12SS06](https://github.com/user-attachments/assets/a90b9b27-4d7f-40c7-8296-6a401d528b05)

Create new tab group to launch all simultaneously.

![12SS07](https://github.com/user-attachments/assets/57935074-bf31-454b-be03-d5239ee49b2d)

*Send group in parallel (last-byte sync)* - send all duplicated requests in the tab group at the same time.

![12SS08](https://github.com/user-attachments/assets/0e89ff9f-12c2-4a83-b759-b850f1d31de3)

Account balance: $-4,500

By duplicating ten requests and sending them in parallel, we are instructing the system to make ten simultaneous requests, each deducting $500 from *tester* account and sending to account 111. 

The application should have processed the first request, locked the database and processed the remaining requests individually

However, due to race condition, the application handles these requests abruptly, resulting in negative balance in tester account and inflated balance in account 111.

**Verifying through source code**

```python
if user['balance'] >= amount:
        conn.execute('UPDATE users SET balance = balance + ? WHERE account_number = ?', 
                     (amount, target_account_number))
        conn.commit()

        conn.execute('UPDATE users SET balance = balance - ? WHERE account_number = ?', 
                     (amount, session['user']))
        conn.commit()
```

Python code that handles fund transfer.

If `user['balance'] >= amount`, the application first updates the recipient's balance with the command `UPDATE users SET balance = balance + ? WHERE account_number = ?`, followed by a commit. Then, it updates the sender’s balance using `UPDATE users SET balance = balance - ? WHERE account_number = ?` and commits again. 

Since these updates are committed separately and not part of a **single atomic transaction**, there’s no locking or proper synchronisation between these operations. This lack of a **transaction or locking mechanism** makes the code vulnerable to race conditions, as concurrent requests could interfere with the balance updates. 

**Fixing race condition**

1. **Atomic Transactions**
    1. Developer should have implemented atomic database transactions to ensure that all steps of fund transfer (deducting and crediting balances) are performed as a single unit
    2. This ensure that either all steps of transaction succeed or none do, preventing partial updates that could lead to an inconsistent state
    
    ```python
    def transfer_funds(amount, target_account_number):
        try:
            with conn:  # Use a transaction context
                # Deduct balance from the sender if sufficient funds exist
                rows_affected = conn.execute(
                    '''
                    UPDATE users
                    SET balance = balance - ?
                    WHERE account_number = ? AND balance >= ?
                    ''',
                    (amount, session['user'], amount)
                ).rowcount
    
                if rows_affected == 0:
                    raise ValueError("Insufficient funds or invalid account")
    
                # Add balance to the target account
                conn.execute(
                    '''
                    UPDATE users
                    SET balance = balance + ?
                    WHERE account_number = ?
                    ''',
                    (amount, target_account_number)
                )
            print("Transaction successful")
        except Exception as e:
            print(f"Transaction failed: {e}")
    ```
    
    1. **Transaction Context (`with conn`)**:
        - Ensures all operations within the block are treated as a single transaction.
        - If an error occurs, all changes are rolled back automatically.
    2. **Balance Check and Update in One Query**:
        - The `balance >= ?` condition in the `UPDATE` query ensures the sender has enough funds before deducting.
    3. **Error Handling**:
        - If no rows are affected by the deduction query, the transaction is aborted with a `ValueError`.
        - Any exception during the transaction triggers an automatic rollback.
    4. **Row Count Validation**:
        - The `rowcount` attribute ensures that the sender's account is updated only if the balance condition is met
2. **Implement Mutex Locks**
    1. Developer could have ensured that only one thread accesses the shared resource (such as account balance) at a time.
    2. Prevent multiple requests from interfering with each other during concurrent transactions
    
    ```python
    import threading
    
    # Create a global lock
    transaction_lock = threading.Lock()
    
    def transfer_funds(amount, target_account_number):
        try:
            # Acquire the lock to ensure exclusive access to the critical section
            with transaction_lock:
                conn = get_database_connection()
    
                # Check sender's balance
                sender_balance = conn.execute(
                    '''
                    SELECT balance 
                    FROM users 
                    WHERE account_number = ?
                    ''',
                    (session['user'],)
                ).fetchone()
    
                if not sender_balance or sender_balance[0] < amount:
                    raise ValueError("Insufficient funds")
    
                # Deduct from sender's account
                conn.execute(
                    '''
                    UPDATE users
                    SET balance = balance - ?
                    WHERE account_number = ?
                    ''',
                    (amount, session['user'])
                )
    
                # Add to recipient's account
                conn.execute(
                    '''
                    UPDATE users
                    SET balance = balance + ?
                    WHERE account_number = ?
                    ''',
                    (amount, target_account_number)
                )
    
                conn.commit()
                print("Transaction successful")
    
        except Exception as e:
            print(f"Transaction failed: {e}")
    ```
    
    - **Global Mutex**:
        - The `transaction_lock` ensures only one thread can execute the `transfer_funds` function at a time.
    - **Thread-Safe Block**:
        - The `with transaction_lock` block guarantees that only one thread can modify the database balances at a time, avoiding race conditions.
    - **Concurrent Threads**:
        - This is particularly useful in multithreaded applications where multiple threads might attempt to access or modify the same data concurrently.

1. **Apply Rate Limits**
    1. Should have implemented rate limiting on critical functions like funds transfers and withdrawals. 
    2. Limit the number of requests processed within a specific time frame, reducing the risk of abuse through rapid, repeated requests
