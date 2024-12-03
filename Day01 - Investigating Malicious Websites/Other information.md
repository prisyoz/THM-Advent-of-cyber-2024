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

`1. bash`

`2. wmic path Win32_ShortcutFile where "Name='your_shortcut.lnk'" get Target`

Replace `your_shortcut.lnk` with the actual name of your shortcut file.

**4. Using a Third-Party Tool**

There are several tools designed to analyze `.lnk` files and display their contents in a user-friendly way:- **LnkParser**: A specialized tool for viewing shortcut files.- **Shortcut Explorer**: Another tool that provides detailed information about shortcuts.

**5. Using PowerShell**

You can also use PowerShell to get detailed information:

`1. $shortcut = (New-Object -ComObject WScript.Shell).CreateShortcut("C:\path\to\your_shortcut.lnk")`

`2. $shortcut.TargetPath` 

`3. $shortcut.Arguments` 

`4. $shortcut.WorkingDirectory`

These methods should help you view the content and properties of a `.lnk` file.

More readings: https://belkasoft.com/forensic-analysis-of-lnk-files
