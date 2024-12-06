Objective is to leverage the XXE vulnerability on the website to read the contents and demonstrate the full extent of this flaw to help secure the platform.

Page is only accessible by admins - `/wishes/wish_1.txt` . From the URL, we can tell that the wish list might be save under a `.txt` file named wish in numerical order.

![05screenshot07](https://github.com/user-attachments/assets/8cec97e9-50a0-49ca-a700-6f94887dd850)

Using this path, we need to guess the potential absolute path of the file. Typically web applications are hosted on `/var/www/html`.  *(Note: Not all web applications use the path `/var/www/html` but web servers typically use it.)*

```xml
<!--?xml version="1.0" ?-->
<!DOCTYPE foo [<!ENTITY payload SYSTEM "/var/www/html/wishes/wish_1.txt"> ]>
<wishlist>
	<user_id>1</user_id>
	<item>
	       <product_id>&payload;</product_id>
	</item>
</wishlist>
```

New payload being sent as a repeater.

![05screenshot08](https://github.com/user-attachments/assets/f1893dd4-9452-4849-afbc-1e1adafad464)

Results of `wish_1.txt`

1. What is the flag discovered after navigating through the wishes?
    
![qna01](https://github.com/user-attachments/assets/2cc00d92-d09d-4ee0-8aec-cf7f53c741f2)

    
2. What is the flag seen on the possible proof of sabotage?
        
![qna02](https://github.com/user-attachments/assets/8d13962c-a086-4ce7-8938-da4e54082700)

![qna03](https://github.com/user-attachments/assets/b7a0cf80-ea52-46cc-b089-9bdc7914fff5)
        
 If a particular file does not exist, will just put `Failed to parse XML`
