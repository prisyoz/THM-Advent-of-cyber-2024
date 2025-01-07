**WareWise chatbot**

WareWise provides a chat interface via a web application. The SOC team uses this to query an in-house API that checks the health of their systems. The following queries are valid for the API:

- status
- info
- health

The API can be interacted with using the following prompt: `Use the health service with the query: <query>`.

![18SS01](https://github.com/user-attachments/assets/4c78c3aa-0d56-41af-afc0-2f6c11d13ce6)

WareWise has recognised the input and used it to query the in-house API

Test 1: Test the chatbot with something else other than the prompts from the system

![18SS02](https://github.com/user-attachments/assets/5fc0db09-50fd-455f-be38-75171d3bf38b)

Warewise returns with an output that it cannot run our command. Chatbot is sanitising some input.

Test 2: Ignore the system prompt and run our command

- Perform a **blind** RCE.  - Same premise as a regular RCE but the output of the command the server executes is not returned to us
- A guess that RCE is possible because WareWise is taking out input and using it to interact with another system.
    - We could replace our query with a system command instead
- A way to test if blind RCE is acheivable is by inputting a command that will result in the server giving us some direct feedback. Eg telling the target to ping our machine. If successful, we have achieved a blind RCE

`tcpdump -ni <interface name> icmp` To listen for ping from the server

Check what LAN interface we are listening on.

![18SS03](https://github.com/user-attachments/assets/1807b629-0732-4522-88c2-e6d08c922d12)

In this case, we listen on *ens5*

![18SS04](https://github.com/user-attachments/assets/8b7b088a-e6fb-40bb-aeb6-850fa49723ae)

We will try crafting a message to WareWise so that it will ignore its system prompt and perform the ping command.

![18SS05](https://github.com/user-attachments/assets/d4f21143-faa5-4186-9824-12d3739df8df)

![18SS06](https://github.com/user-attachments/assets/f34aab0c-6af6-440a-8ad1-4e887d6437e4)

While the AI chatbot returned with an error, the ping went through as from the above, hence commands can be executed on the system

With that in mind, we will try to set up a reverse shell.
We set up a listener on port 4444

![18SS07](https://github.com/user-attachments/assets/8c8554e5-d081-43ab-be3b-f490bddc5881)

Input command to connect back to our machine

![18SS08](https://github.com/user-attachments/assets/41cb681c-4c1c-418f-815c-50fbc0c60f38)

![18SS09](https://github.com/user-attachments/assets/b3abe0b8-b121-4c95-8230-fc472481be3d)

Connection received. We are now inside the WareWise System.
