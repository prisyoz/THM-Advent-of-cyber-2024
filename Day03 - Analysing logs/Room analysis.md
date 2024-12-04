Example: Known attack of a web shell being uploaded to Frosty Pines Resort between 1130 and 1200 on October 3rd 2024.

![screenshot1](https://github.com/user-attachments/assets/ac343151-8984-4b69-afa7-12c68abd7d16)

There is a total of 483 hits between on 3 October 2024, all hits are during the time 1130 - 1200.

![screenshot2](https://github.com/user-attachments/assets/d10f9266-b576-4f5c-806c-4e910c7dbceb)

2 IP addresses were found during this time period. `10.9.254.186` and `10.11.83.34`.

![screenshot3_1](https://github.com/user-attachments/assets/58ffd7c6-02d9-4215-9d8b-f016ec4b846b)

A run through on IP address `10.9.254.186` seems to be clean.

![screenshot3](https://github.com/user-attachments/assets/3231d4ce-ed9d-4605-a714-bad8ea043dfa)

Whereas on `10.11.83.34` , suspicious commands seemed to be ran after `shell.php` was uploaded onto the server.

Knowing that, we then attempt to recreate the events that may have occurred. A look on the website brings us to a login page. Knowing that there seems to be no brute-force attack as seen on the ELK logs, the attacker might have gone in through weak credentials.

![screenshot4](https://github.com/user-attachments/assets/7621fe3a-3878-47ad-b68d-5ccab30f270b)

![screenshot5](https://github.com/user-attachments/assets/6d3c4bc7-4e3f-43de-af3c-1096d3e388a8)

We tried with a common email address for administrators to find out that this does not exist and it is written explicitly there.

![screenshot5_1](https://github.com/user-attachments/assets/7915c589-5fe6-4bcf-8258-ad2b1c320ddc)

Then we tried another common admin email address and the page says that it has an incorrect password, which means that `admin@frostypines.thm` does exist on the system. We then tried a couple of other common weak passwords and managed to get into the system. 

(Note: For failed logins, it might be better to put incorrect email/password so that attackers do not know which of the boxes are correct.)

![screenshot6](https://github.com/user-attachments/assets/d3ccb1ae-20b3-4bb2-b99f-b836c5efe55a)

Knowing from the ELK logs, the attacker uploaded a file named shell.php and in the admin account, we found out there is a site where we can upload files. 

![screenshot7](https://github.com/user-attachments/assets/ed4ddbca-19fb-4195-81ac-c20d6ff57dd1)

We tried uploading shell.php by adding a new room.

![screenshot7_1](https://github.com/user-attachments/assets/bb9dcc97-83e7-4b50-b6e7-e41d6e1f70e8)

And it was a success! Which meant that shell.php has been successfully uploaded. 

Referring back to the logs where we found out the directory that the attacker had executed the commands from, we tried them in the browser and it appears with a address bar.

![screenshot8](https://github.com/user-attachments/assets/60b2a224-7e88-48b4-8e24-13cfd1321d1f)

We tried keying some commands into the address bar and found that we are able to access the directory. 

![test14](https://github.com/user-attachments/assets/3bc6b357-0083-461a-8b22-47280aa65120)

Someone uploaded another sh3ll.php onto the server as well. Maybe there were 2 attackers, but logs at ELK showed nothing. Perhaps it was uploaded from way long ago. 

**Other information that was found through exploiting the vulnerablity**

`cat /etc/passwd` We can see  the user accounts on the system

![test4](https://github.com/user-attachments/assets/3827c0ec-4b7b-4f32-b90e-e8021e75e0a7)

`whoami` shows the host of the site, which is www-data

![test2](https://github.com/user-attachments/assets/c77bab7a-6ef4-4f3d-a729-ab0ddb5fbf67)

No permission on `/etc/shadow`

![test5](https://github.com/user-attachments/assets/bec29bf3-c2d1-4679-8a2b-32a4ffb0583a)

`id` shows the current user who has sudo privilege.

![test6](https://github.com/user-attachments/assets/12dd4b1b-b486-4d27-bea8-071aa6dd8059)

`hostnamectl` displays the operating system which allows the attacker to potentially exploit the system

![test8](https://github.com/user-attachments/assets/3457f722-9f57-415e-84f9-c9a933332155)

`ip a` displays information about all network interfaces on the system

![test11](https://github.com/user-attachments/assets/34c778dd-f96a-4ca2-b5e5-321646b6e7ce)

We are also able to navigate to the users page where we are able to see the account details. We can grab hold of the admin details, inclusive of first and last names, date of birth, phone number and email addresses. 

In a real world situation, this page might contain details of clients who have created an account and may have booked rooms through the website. Attackers may be able to conduct phishing, identity theft etc after gaining access to the website.

![test1](https://github.com/user-attachments/assets/20a1c2b1-c3f3-4010-a0af-b40fe3ba9fd2)
