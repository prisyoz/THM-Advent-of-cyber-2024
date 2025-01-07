We have a suspected compromised machine that was from Marta May Ware’s computer, the latest victim of Mayor Malware’s long-running schemes.

Marta May Ware IP address - `10.10.229.217`

![20SS01](https://github.com/user-attachments/assets/22fa24c7-84da-4077-bdf6-86ee498b5fa2)

Scrolling through, we note several interesting packets as outlined in the above screenshot.

Clicking on frame 440, we notice a message that says “I am in Mayor!”

![20SS02](https://github.com/user-attachments/assets/4e05c5c8-97bb-45b1-9676-6ec7cdf35bbc)

Hence, to find out more about this communication session, we follow the HTTP stream and found out that there is a reply from the IP address `10.10.123.224`

![20SS03](https://github.com/user-attachments/assets/0f017cc0-cb09-498f-b5be-7d4859f321a6)

We then take a look at another stream with the info `GET /command HTTP/1.1` and find out that it has the same destination IP of `10.10.123.224` just like the previous stream. Following the stream, we see another reponse `whoami`, which is a Windows and Linux command to display the current user’s information. Most likely, the attacker is  trying to gather information from the compromised machine.

![20SS04](https://github.com/user-attachments/assets/b6399dfc-8336-4fa2-9105-6015ff148eb5)

Continuing the same IP source address, we then find other packets, consisting of `exfiltrate`and `beacon`

![20SS05](https://github.com/user-attachments/assets/533f4ec6-4d07-4232-bc15-40a4dbcd80aa)

From the `exfilitrate` stream, we managed to capture a packet with an encrypted key.

AES ECB is your chance to decrypt the encrypted beacon with the key: 1234567890abcdef1234567890abcdef
--f5964f77-daf1-4853-aacb-df4754eaacaf--

We can also see that the filename that the C2 server exfiltrated is `credentials.txt`

![20SS08](https://github.com/user-attachments/assets/8046de57-e3f1-44e2-9677-d7f690fc2b40)

In the `beacon` stream, we then find the encrypted code. 

![20SS07](https://github.com/user-attachments/assets/73b0787d-84e2-4eba-b312-15e453dbe257)

Encrypted key: 8724670c271adffd59447552a0ef3249

We then attempt to decrypt the encrypted key with the known key to gain the message `THM_Secret_101`
![20SS09](https://github.com/user-attachments/assets/359f1578-c838-43e6-9071-2042ea43546b)
