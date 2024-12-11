Security is as strong as the weakest link. It is easier to convince a user to open an “important” document than exploiting a patched system behind a firewall. Hence, “human hacking” is usually the easiest to accomplish and falls under social engineering.

Phishing works by sending a “bait” to a usually large group of target users. The attacker often craft their messages with a sense of urgency, prompting target users to take immediate action without thinking critically, increasing the chances of success. The purpose is to steal personal information or install malware, usually by convincing the target user to fill out a form, open a file or click a link. Attacker just need to have their target users open the malicious file or view the malicious link and will be able to trigger specific actions that would give the attack control over the user’s system.

**Macros**

A macro refers to a set of programmed instructions designed to automate repetitive tasks. MS Word, among other MS Office products, supports adding macros to documents. But these automated programs can be hijacked for malicious purposes.

To add a macro to an MS Word document, click on `View` menu, select `Macros` , specify name of macro and specify to save it in current document. Press `Create` 

**Attack Plan**

Create a document with a malicious macro so that upon opening of the document, the macro will execute a payload and connect to the attacker’s machine, giving remote control. The attacker needs to ensure that he is listening for incoming connections on his machine before emailing the malicious document. By executing the macro, the attacker gains remote access to the user’s system through a reverse shell, allowing him to execute commands and control the machine remotely.

Steps:

1. Create a document with a malicious macro
2. Start listening for incoming connections on the attacker’s system
3. Email the document and wait for the target user to open it
4. Target opens the document and connects to the attacker’s system
5. Control the target’s system
