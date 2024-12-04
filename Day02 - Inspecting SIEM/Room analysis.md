According to the alert sent by the Mayor's office, the activity occurred on Dec 1st, 2024, between 0900 and 0930. 

Check the activity during the time of alert

![screenshot1](https://github.com/user-attachments/assets/b1dc64cd-15b7-437a-be26-a1738347fede)

21 events occur during the alert activity

Activity might happen before the attack 

Dates change 29 nov - 1 Dec 2024

![screenshot2](https://github.com/user-attachments/assets/de494806-7794-4f89-9934-965c00e4ed20)

6,814 events happened with a spike on 1 Dec when the attack happen. Source IP 10.0.11.11. `Event.category`: Authentication `Event.outcome`: success

![screenshot3](https://github.com/user-attachments/assets/e6662dab-72ef-42a7-85da-d1d6a984dd3e)

A closer look on 10.0.11.11 shows authentication success coming from the same user and same IP address. But no explanation on the spike. (the graph chart seems normal)

Filter out 10.0.11.11

![screenshot4_1](https://github.com/user-attachments/assets/09444c3d-ef6e-4790-9da8-8360e1c5577b)

![screenshot5](https://github.com/user-attachments/assets/02371850-0789-401d-8a30-d29f0fab2ed7)

Many `event.outcome`: failure before reaching a `event.outcome`: success. Source IP address 10.0.255.1. Might be someone trying to brute force on 1 dec. Results of authentication failure is due to user logon failure.

![screenshot6](https://github.com/user-attachments/assets/38f8ae82-db83-4d51-a92d-7b7dea36ab70)

Successful brute-force attack on different machines.

![screenshot7](https://github.com/user-attachments/assets/35d068e7-5504-443b-9211-6b0f1f670efc)

![screenshot8](https://github.com/user-attachments/assets/ad9b7053-117e-4320-82fc-52008ef102c0)

After succeeding with brute-force attempt  because of successful authentication attempt, they quickly ran some PowerShell commands on the different affected machines. After PowerShell commands were run, no further login attempts

Further investigation into PowerShell commands

![process command line](https://github.com/user-attachments/assets/38df47be-5cdd-4b7e-a471-355513ef267b)

```jsx
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -EncodedCommand SQBuAHMAdABhAGwAbAAtAFcAaQBuAGQAbwB3AHMAVQBwAGQAYQB0AGUAIAAtAEEAYwBjAGUAcAB0AEEAbABsACAALQBBAHUAdABvAFIAZQBiAG8AbwB0AA==
```

Commands encrypted in base64. To decrypt it

![screenshot9](https://github.com/user-attachments/assets/7877c849-7048-476c-ba68-c780c062da23)

`Install-WindowsUpdate -AcceptAll -AutoReboot`automate the installation of Windows updates via PowerShell. This specific command is typically provided by third-party PowerShell modules, such as **PSWindowsUpdate**, which enables administrators to manage Windows updates on local or remote systems.

![screenshot11](https://github.com/user-attachments/assets/f43d023a-7022-42fa-ab9e-1daba5fbb5cd)

Lastly, finding out that ADM-01 is a friend and fixed the credentials
