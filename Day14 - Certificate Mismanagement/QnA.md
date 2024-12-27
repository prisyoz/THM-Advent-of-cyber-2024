![14SS02.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e2c2e96-4835-4340-8f72-9fd2a848f618/8a6066c7-f85b-474a-a492-5f3bd039bcc5/14SS02.png)

Self-signed cert

**MITM attack (Burpsuite)**

Setting up a listener through Burpsuite

![14SS04](https://github.com/user-attachments/assets/a40af5c9-972a-4644-988b-af3274b6fddb)

Burp Suite comes with a self-signed certificate, in which users are prompted to accept it without thinking of verifying the certificate origin first

Setting up machine as a gateway which will divert all of the traffic, usually routed through the legitimate gateway to the own machine.

**Note:** In practice, the adversary can launch a similar attack if they can control the user’s gateway and their attack can easily succeed against websites not using properly signed certificates. This attack requires more than adding an entry into the `/etc/hosts` file; however, this task aims to emulate parts of the attack.

What is the name of the CA that has signed the Gift Scheduler certificate?

![14SS08](https://github.com/user-attachments/assets/edbde604-af83-483a-86db-87674d3a4098)

Look inside the POST requests in the HTTP history. What is the password for the `snowballelf` account?

![14SS07](https://github.com/user-attachments/assets/bd08507d-6ed8-48b5-8307-e712b1bdeb32)

Use the credentials for any of the elves to authenticate to the Gift Scheduler website. What is the flag shown on the elves’ scheduling page?

![14SS09](https://github.com/user-attachments/assets/c27c34d1-b0c2-4f11-b755-fac890c5eb12)

![14SS10](https://github.com/user-attachments/assets/4b504d06-806e-4be3-a1a5-f130aa0e39aa)

What is the password for Marta May Ware’s account?

![14SS06](https://github.com/user-attachments/assets/cd60ba98-5d7d-4e7f-b71d-bd6f6163ac3e)

username=marta_mayware&password=H0llyJ0llySOCMAS!

Mayor Malware finally succeeded in his evil intent: with Marta May Ware’s username and password, he can finally access the administrative console for the Gift Scheduler. G-Day is cancelled!

What is the flag shown on the admin page?

![14SS11](https://github.com/user-attachments/assets/67e1f94b-25a8-45b8-a63d-1c454c8f03de)
