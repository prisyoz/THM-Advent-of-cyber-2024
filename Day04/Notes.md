**Detection Gaps**

Detection gaps are usually for one of two main reasons:

- Security is a cat-and-mouse game.
    - As we detect more, threat actors and red teamers will find new sneaky ways to thwart our detection. We then need to study more novel techniques and update our signature and alert rules to detect these new techniques
- The line between anomalous and expected behaviour is often very fine and sometimes has significant overlap.
    - E.g unusual log in IP address and time, which may signify somebody travelling overseas despite being a local company

**Cyber kill chain**

![Cyber kill chain](https://github.com/user-attachments/assets/253481f6-605b-4cd6-86ff-a7980677ff67)

It is most ideal to prevent all attacks at the start of the kill chain, but it is not possible. 

If unable to fully detect and prevent a threat actor at any one phase in the kill chain, the goal becomes to perform detections across the entire kill chain in such a way that even if there are detection gaps in a single phase, the gap is covered in a later phase. 

The goal is to ensure we can detect the threat actor before the very last phase of goal execution.

**MITRE ATT&CK**

A popular framework for understanding the different techniques and tactics that threat actors perform through the kill chain. The framework is a collection of tactics, techniques, and procedures”(TTPs) that have been seen to be implement by real threat actors. It provides a navigator tool where these TTPs can be investigated

![Mitre attack](https://github.com/user-attachments/assets/acd66815-6ee3-4b9a-82c3-0e4edb96482f)

But it is more theory, hence we have

**Atomic Red**

The Atomic Red Team library is a collection of red team test cases that are mapped to the MITRE ATT&CK framework. The library consists of simple test cases that can be executed by any blue team to test for detection gaps and help close them down. It also supports automation, where techniques can be automatically executed. It can also be executed manually.

**Running an atomic (Tutorial walkthrough)**

Suspected *T1566.001 Spearfishing* with an attachment

- PowerShell prompt as administrator

`Get-Help Invoke-Atomictest` for the help page

![05screenshot01](https://github.com/user-attachments/assets/fb86f94c-e098-4035-8a7c-cbe2046fb3be)

| Parameter | Explanation | Example use |
| --- | --- | --- |
| **`-Atomic Technique`** | This defines what technique you want to emulate. You can use the complete technique name or the "TXXXX" value. This flag can be omitted. | **`Invoke-AtomicTest -AtomicTechnique T1566.001`** |
| **`-ShowDetails`** | Shows the details of each test included in the Atomic. | **`Invoke-AtomicTest T1566.001 -ShowDetails`** |
| **`-ShowDetailsBrief`** | Shows the title of each test included in the Atomic. | **`Invoke-AtomicTest T1566.001 -ShowDetailsBrief`** |
| **`-CheckPrereqs`** | Provides a check if all necessary components are present for testing | **`Invoke-AtomicTest T1566.001 -CheckPrereqs`** |
| **`-TestNames`** | Sets the tests you want to execute using the complete Atomic Test Name. | **`Invoke-AtomicTest T1566.001 -TestNames "Download Macro-Enabled Phishing Attachment"`** |
| **`-TestGuids`** | Sets the tests you want to execute using the unique test identifier. | **`Invoke-AtomicTest T1566.001 -TestGuids 114ccff9-ae6d-4547-9ead-4cd69f687306`** |
| **`-TestNumbers`** | Sets the tests you want to execute using the test number. The scope is limited to the Atomic Technique. | **`Invoke-AtomicTest T1566.001 -TestNumbers 2,3`** |
| **`-Cleanup`** | Run the cleanup commands that were configured to revert your machine state to normal. | **`Invoke-AtomicTest T1566.001 -TestNumbers 2 -Cleanup`** |

Showed 2 tests

1. Macro-Enabled Phishing Attachment
2. Word pawned a command shell and used IP address in command line

Contains description, attack commands, dependencies and cleanup commands

![05screenshot03](https://github.com/user-attachments/assets/afbbda01-932b-4ff2-9abb-13d306f738ad)

![05screenshot04](https://github.com/user-attachments/assets/c6d601f9-df10-4128-a56f-e3d535178486)

| **Key** | **Value** | **Description** |
| --- | --- | --- |
| Technique | Phishing: Spearphishing Attachment T1566.001 | The full name of the MITRE ATT&CK technique that will be tested |
| Atomic Test Name | Download Macro-Enabled Phishing Attachment | A descriptive name of the type of test that will be executed |
| Atomic Test Number | 1 | A number is assigned to the test; we can use this in the command to specify which test we want to run. |
| Atomic Test GUID | 114ccff9-ae6d-4547-9ead-4cd69f687306 | A unique ID is assigned to this test; we can use this in the command to specify which test we want to run. |
| Description | This atomic test downloads a macro-enabled document from the Atomic Red Team GitHub repository, simulating an end-user clicking a phishing link to download the file. The file "PhishingAttachment.xlsm" is downloaded to the %temp% directory. | Provides a detailed explanation of what the test will do. |
| Attack commands | **Executor:** powershell
**ElevationRequired:** False
**Command:** $url = ‘http://localhost/PhishingAttachment.xlsm’ Invoke-WebRequest -Uri $url -OutFile $env:TEMP.xlsm | This provides an overview of all the commands run during the test, including the executor of those commands and the required privileges. It also helps us determine where to look for artefacts in Windows Event Viewer. |
| Cleanup commands | Command: Remove-Item $env:TEMP.xlsm -ErrorAction Ignore | An overview of the commands executed to revert the machine back to its original state. |
| Dependencies | There are no dependencies required. | An overview of all required resources that must be present on the testing machine in order to execute the test |

`Invoke-AtomicTest <Mitre ID No> -TestNumbers <no> -CheckPrereq` to check if all required resources are in place to conduct the emulation successfully

![05screenshot05](https://github.com/user-attachments/assets/9142edfe-ddeb-4d10-b5f7-5b7fc8d919b1)

TestNumbers 1 do not need additionally dependencies (Orange)

TestNumbers 2 needed Microsoft Word to be installed (Pink)

`Invoke-AtomicTest <Mitre ID> -TestNumbers <no>` to run the emulation

![05screenshot06](https://github.com/user-attachments/assets/9f16fa8d-05a8-47cc-bc4c-210da2467bf4)

Based on this output, it means that the test was successfully executed and we can now analyse the logs in the Windows Event Viewer to find the indicators of the Attack and Compromise.

**Detecting the Atomic**

We can look for log entries that point us to this emulated attack through Windows Event Logs, which comes with Sysmon installed. System Monitor (Sysmon) provides analysts with detailed information about process creation, network connections and changes to file creation time.

![05screenshot08](https://github.com/user-attachments/assets/47d308b2-f9ae-462f-9fda-588c7d922f8f)

Note that a `.xlsm` file was created

Navigating to the directory that the file was saved into, we can find the files that were downloaded.

![05screenshot09_1](https://github.com/user-attachments/assets/440972ae-8ad8-41e9-89b0-0461ab66fdc8)

`Invoke-AtomicTest <Mitre ID> -TestNumbers <no> -cleanup` to clean up files from previous tests

![05screenshot07](https://github.com/user-attachments/assets/044423c5-e2f3-4427-bf07-529f40e396e9)

**Alerting on the Atomic**

As we found multiple indicators of compromise through the Sysmon event log. We can use this information to create detection rules to include in our EDR, SIEM, IDS etc. These tools offer functionalities that allow analysts to import custom detection rules in formats such as Yara, Sigma, Snort etc.

Looking at the command line `"powershell.exe" & {$url = 'http://localhost/PhishingAttachment.xlsm' $url2 = 'http://localhost/PhishingAttachment.txt' Invoke-WebRequest -Uri $url -OutFile $env:TEMP\PhishingAttachment.xlsm Invoke-WebRequest -Uri $url2 -OutFile $env:TEMP\PhishingAttachment.txt}`

Analysing each command

- `Invoke-WebReqest` - uncommon for this command to run from a script behind the scenes
- `$url = 'http://localhost/PhishingAttachment.xlsm` - Attackers often use a specific malicious domain to host their payloads, including malicious URL in the Signma rule could help us detect that specific URL.
- `PhishingAttachment.xlsm` malicious payload download and saved on the system.

![05screenshot10](https://github.com/user-attachments/assets/80656bc6-ecf4-4736-848c-1c2bd44dbd09)
