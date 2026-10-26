---
tags:
  - ИБ
  - ATT&CK
aliases:
  - Каталог техник ATT&CK
  - Список техник ATT&CK
  - Techniques catalog
---
# Каталог техник ATT&CK: Enterprise v20.0

Полный перечень техник матрицы Enterprise в **ATT&CK v20.0** (13 октября 2026): **227 техник и 479 подтехник**. Техники сгруппированы по тактикам. Если техника относится к нескольким тактикам, она стоит в основной (первой по матрице) и перечислена в колонке «Также в тактиках» — таких техник 32.

Как читать: ID ведёт на страницу техники на attack.mitre.org, подтехники записаны в формате `Txxxx.yyy`. Платформы, процедурные примеры и детекты — на странице техники; методика разбора — в [[Techniques]]; описание тактик — в [[Att&ck]].

## Сколько техник в каждой тактике

| Тактика | ID | Техник | Подтехник в столбце |
| --- | --- | --- | --- |
| Reconnaissance | TA0043 | 12 | 34 |
| Resource Development | TA0042 | 9 | 42 |
| Initial Access | TA0001 | 11 | 11 |
| Execution | TA0002 | 22 | 44 |
| Persistence | TA0003 | 22 | 91 |
| Privilege Escalation | TA0004 | 13 | 83 |
| Stealth | TA0005 | 31 | 120 |
| Defense Impairment | TA0112 | 18 | 39 |
| Credential Access | TA0006 | 18 | 50 |
| Discovery | TA0007 | 34 | 15 |
| Lateral Movement | TA0008 | 9 | 14 |
| Collection | TA0009 | 17 | 24 |
| Command and Control | TA0011 | 18 | 27 |
| Exfiltration | TA0010 | 10 | 10 |
| Impact | TA0040 | 15 | 18 |
| **Итого карточек в матрице** | — | **259** | **622** |

Сумма карточек (259) больше числа уникальных техник (227), потому что 32 техники относятся к нескольким тактикам. Уникальных подтехник — 479; в столбцах они повторяются вместе с техниками.

## TA0043 Reconnaissance

| ID | Техника | Подтехники | Также в тактиках |
| --- | --- | --- | --- |
| T1595 | Active Scanning | ..001 Scanning IP Blocks; ..002 Vulnerability Scanning; ..003 Wordlist Scanning | — |
| T1592 | Gather Victim Host Information | ..001 Hardware; ..002 Software; ..003 Firmware; ..004 Client Configurations | — |
| T1589 | Gather Victim Identity Information | ..001 Credentials; ..002 Email Addresses; ..003 Employee Names | — |
| T1590 | Gather Victim Network Information | ..001 Domain Properties; ..002 DNS; ..003 Network Trust Dependencies; ..004 Network Topology; ..005 IP Addresses; ..006 Network Security Appliances | — |
| T1591 | Gather Victim Org Information | ..001 Determine Physical Locations; ..002 Business Relationships; ..003 Identify Business Tempo; ..004 Identify Roles | — |
| T1598 | Phishing for Information | ..001 Spearphishing Service; ..002 Spearphishing Attachment; ..003 Spearphishing Link; ..004 Spearphishing Voice | — |
| T1682 | Query Public AI Services | — | — |
| T1597 | Search Closed Sources | ..001 Threat Intel Vendors; ..002 Purchase Technical Data | — |
| T1596 | Search Open Technical Databases | ..001 DNS/Passive DNS; ..002 WHOIS; ..003 Digital Certificates; ..004 CDNs; ..005 Scan Databases | — |
| T1593 | Search Open Websites/Domains | ..001 Social Media; ..002 Search Engines; ..003 Code Repositories | — |
| T1681 | Search Threat Vendor Data | — | — |
| T1594 | Search Victim-Owned Websites | — | — |

## TA0042 Resource Development

| ID | Техника | Подтехники | Также в тактиках |
| --- | --- | --- | --- |
| T1650 | Acquire Access | — | — |
| T1583 | Acquire Infrastructure | ..001 Domains; ..002 DNS Server; ..003 Virtual Private Server; ..004 Server; ..005 Botnet; ..006 Web Services; ..007 Serverless; ..008 Malvertising | — |
| T1586 | Compromise Accounts | ..001 Social Media Accounts; ..002 Email Accounts; ..003 Cloud Accounts | — |
| T1584 | Compromise Infrastructure | ..001 Domains; ..002 DNS Server; ..003 Virtual Private Server; ..004 Server; ..005 Botnet; ..006 Web Services; ..007 Serverless; ..008 Network Devices | — |
| T1587 | Develop Capabilities | ..001 Malware; ..002 Code Signing Certificates; ..003 Digital Certificates; ..004 Exploits | — |
| T1585 | Establish Accounts | ..001 Social Media Accounts; ..002 Email Accounts; ..003 Cloud Accounts | — |
| T1683 | Generate Content | ..001 Written Content; ..002 Audio-Visual Content | — |
| T1588 | Obtain Capabilities | ..001 Malware; ..002 Tool; ..003 Code Signing Certificates; ..004 Digital Certificates; ..005 Exploits; ..006 Vulnerabilities; ..007 Artificial Intelligence | — |
| T1608 | Stage Capabilities | ..001 Upload Malware; ..002 Upload Tool; ..003 Install Digital Certificate; ..004 Drive-by Target; ..005 Link Target; ..006 SEO Poisoning; ..007 AI Result Poisoning | — |

## TA0001 Initial Access

| ID | Техника | Подтехники | Также в тактиках |
| --- | --- | --- | --- |
| T1659 | Content Injection | — | Command and Control |
| T1189 | Drive-by Compromise | — | — |
| T1190 | Exploit Public-Facing Application | — | — |
| T1133 | External Remote Services | — | Persistence |
| T1200 | Hardware Additions | — | — |
| T1566 | Phishing | ..001 Spearphishing Attachment; ..002 Spearphishing Link; ..003 Spearphishing via Service; ..004 Spearphishing Voice | — |
| T1091 | Replication Through Removable Media | — | Lateral Movement |
| T1195 | Supply Chain Compromise | ..001 Compromise Software Dependencies and Development Tools; ..002 Compromise Software Supply Chain; ..003 Compromise Hardware Supply Chain | — |
| T1199 | Trusted Relationship | — | — |
| T1078 | Valid Accounts | ..001 Default Accounts; ..002 Domain Accounts; ..003 Local Accounts; ..004 Cloud Accounts | Persistence, Privilege Escalation, Stealth |
| T1669 | Wi-Fi Networks | — | — |

## TA0002 Execution

| ID | Техника | Подтехники | Также в тактиках |
| --- | --- | --- | --- |
| T1197 | BITS Jobs | — | Persistence, Stealth |
| T1651 | Cloud Administration Command | — | — |
| T1059 | Command and Scripting Interpreter | ..001 PowerShell; ..002 AppleScript; ..003 Windows Command Shell; ..004 Unix Shell; ..005 Visual Basic; ..006 Python; ..007 JavaScript; ..008 Network Device CLI; ..009 Cloud API; ..010 AutoHotKey & AutoIT; ..011 Lua; ..012 Hypervisor CLI; ..013 Container CLI/API | — |
| T1609 | Container Administration Command | — | — |
| T1610 | Deploy Container | — | — |
| T1699 | Direct Prompt Injection | — | — |
| T1675 | ESXi Administration Command | — | — |
| T1203 | Exploitation for Client Execution | — | — |
| T1574 | Hijack Execution Flow | ..001 DLL; ..004 Dylib Hijacking; ..005 Executable Installer File Permissions Weakness; ..006 Dynamic Linker Hijacking; ..007 Path Interception by PATH Environment Variable; ..008 Path Interception by Search Order Hijacking; ..009 Path Interception by Unquoted Path; ..010 Services File Permissions Weakness; ..011 Services Registry Permissions Weakness; ..012 COR\_PROFILER; ..013 KernelCallbackTable; ..014 AppDomainManager | Stealth |
| T1698 | Indirect Prompt Injection | — | — |
| T1674 | Input Injection | — | — |
| T1559 | Inter-Process Communication | ..001 Component Object Model; ..002 Dynamic Data Exchange; ..003 XPC Services | — |
| T1106 | Native API | — | — |
| T1677 | Poisoned Pipeline Execution | — | — |
| T1053 | Scheduled Task/Job | ..002 At; ..003 Cron; ..005 Scheduled Task; ..006 Systemd Timers; ..007 Container Orchestration Job | Persistence, Privilege Escalation |
| T1648 | Serverless Execution | — | — |
| T1129 | Shared Modules | — | — |
| T1072 | Software Deployment Tools | — | Lateral Movement |
| T1569 | System Services | ..001 Launchctl; ..002 Service Execution; ..003 Systemctl | — |
| T1127 | Trusted Developer Utilities Proxy Execution | ..001 MSBuild; ..002 ClickOnce; ..003 JamPlus | Stealth |
| T1204 | User Execution | ..001 Malicious Link; ..002 Malicious File; ..003 Malicious Image; ..004 Malicious Copy and Paste; ..005 Malicious Library | — |
| T1047 | Windows Management Instrumentation | — | — |

## TA0003 Persistence

| ID | Техника | Подтехники | Также в тактиках |
| --- | --- | --- | --- |
| T1098 | Account Manipulation | ..001 Additional Cloud Credentials; ..002 Additional Email Delegate Permissions; ..003 Additional Cloud Roles; ..004 SSH Authorized Keys; ..005 Device Registration; ..006 Additional Container Cluster Roles; ..007 Additional Local or Domain Groups | Privilege Escalation |
| T1547 | Boot or Logon Autostart Execution | ..001 Registry Run Keys / Startup Folder; ..002 Authentication Package; ..003 Time Providers; ..004 Winlogon Helper DLL; ..005 Security Support Provider; ..006 Kernel Modules and Extensions; ..007 Re-opened Applications; ..008 LSASS Driver; ..009 Shortcut Modification; ..010 Port Monitors; ..012 Print Processors; ..013 XDG Autostart Entries; ..014 Active Setup; ..015 Login Items | Privilege Escalation |
| T1037 | Boot or Logon Initialization Scripts | ..001 Logon Script (Windows); ..002 Login Hook; ..003 Network Logon Script; ..004 RC Scripts; ..005 Startup Items | Privilege Escalation |
| T1671 | Cloud Application Integration | — | — |
| T1554 | Compromise Host Software Binary | — | — |
| T1136 | Create Account | ..001 Local Account; ..002 Domain Account; ..003 Cloud Account | — |
| T1543 | Create or Modify System Process | ..001 Launch Agent; ..002 Systemd Service; ..003 Windows Service; ..004 Launch Daemon; ..005 Container Service | Privilege Escalation |
| T1546 | Event Triggered Execution | ..001 Change Default File Association; ..002 Screensaver; ..003 Windows Management Instrumentation Event Subscription; ..004 Unix Shell Configuration Modification; ..005 Trap; ..006 LC\_LOAD\_DYLIB Addition; ..007 Netsh Helper DLL; ..008 Accessibility Features; ..009 AppCert DLLs; ..010 AppInit DLLs; ..011 Application Shimming; ..012 Image File Execution Options Injection; ..013 PowerShell Profile; ..014 Emond; ..015 Component Object Model Hijacking; ..016 Installer Packages; ..017 Udev Rules; ..018 Python Startup Hooks | Privilege Escalation |
| T1668 | Exclusive Control | — | — |
| T1525 | Implant Internal Image | — | — |
| T1556 | Modify Authentication Process | ..001 Domain Controller Authentication; ..002 Password Filter DLL; ..003 Pluggable Authentication Modules; ..004 Network Device Authentication; ..005 Reversible Encryption; ..006 Multi-Factor Authentication; ..007 Hybrid Identity; ..008 Network Provider DLL; ..009 Conditional Access Policies | Defense Impairment, Credential Access |
| T1112 | Modify Registry | — | Defense Impairment |
| T1137 | Office Application Startup | ..001 Office Template Macros; ..002 Office Test; ..003 Outlook Forms; ..004 Outlook Home Page; ..005 Outlook Rules; ..006 Add-ins | — |
| T1653 | Power Settings | — | — |
| T1542 | Pre-OS Boot | ..001 System Firmware; ..002 Component Firmware; ..003 Bootkit; ..004 ROMMONkit; ..005 TFTP Boot | Stealth |
| T1505 | Server Software Component | ..001 SQL Stored Procedures; ..002 Transport Agent; ..003 Web Shell; ..004 IIS Components; ..005 Terminal Services DLL; ..006 vSphere Installation Bundles | — |
| T1176 | Software Extensions | ..001 Browser Extensions; ..002 IDE Extensions | — |
| T1205 | Traffic Signaling | ..001 Port Knocking; ..002 Socket Filters | Stealth, Command and Control |

## TA0004 Privilege Escalation

| ID | Техника | Подтехники | Также в тактиках |
| --- | --- | --- | --- |
| T1548 | Abuse Elevation Control Mechanism | ..001 Setuid and Setgid; ..002 Bypass User Account Control; ..003 Sudo and Sudo Caching; ..004 Elevated Execution with Prompt; ..005 Temporary Elevated Cloud Access; ..006 TCC Manipulation | — |
| T1134 | Access Token Manipulation | ..001 Token Impersonation/Theft; ..002 Create Process with Token; ..003 Make and Impersonate Token; ..004 Parent PID Spoofing; ..005 SID-History Injection | Stealth |
| T1484 | Domain or Tenant Policy Modification | ..001 Group Policy Modification; ..002 Trust Modification | Defense Impairment |
| T1611 | Escape to Host | — | — |
| T1068 | Exploitation for Privilege Escalation | — | — |
| T1055 | Process Injection | ..001 Dynamic-link Library Injection; ..002 Portable Executable Injection; ..003 Thread Execution Hijacking; ..004 Asynchronous Procedure Call; ..005 Thread Local Storage; ..008 Ptrace System Calls; ..009 Proc Memory; ..011 Extra Window Memory Injection; ..012 Process Hollowing; ..013 Process Doppelgnging; ..014 VDSO Hijacking; ..015 ListPlanting | Stealth |

## TA0005 Stealth

| ID | Техника | Подтехники | Также в тактиках |
| --- | --- | --- | --- |
| T1696 | Anti-Automation Evasion | ..001 Automate CAPTCHA Completion; ..002 Avoid CAPTCHA | — |
| T1612 | Build Image on Host | — | — |
| T1622 | Debugger Evasion | — | Discovery |
| T1678 | Delay Execution | — | — |
| T1140 | Deobfuscate/Decode Files or Information | — | — |
| T1006 | Direct Volume Access | — | — |
| T1480 | Execution Guardrails | ..001 Environmental Keying; ..002 Mutual Exclusion | — |
| T1211 | Exploitation for Stealth | — | — |
| T1564 | Hide Artifacts | ..001 Hidden Files and Directories; ..002 Hidden Users; ..003 Hidden Window; ..004 NTFS File Attributes; ..005 Hidden File System; ..006 Run Virtual Instance; ..007 VBA Stomping; ..008 Email Hiding Rules; ..009 Resource Forking; ..010 Process Argument Spoofing; ..011 Ignore Process Interrupts; ..012 File/Path Exclusions; ..013 Bind Mounts; ..014 Extended Attributes | — |
| T1070 | Indicator Removal | ..003 Clear Command History; ..004 File Deletion; ..005 Network Share Connection Removal; ..006 Timestomp; ..007 Clear Network Connection History and Configurations; ..008 Clear Mailbox Data; ..009 Clear Persistence; ..010 Relocate Malware | — |
| T1202 | Indirect Command Execution | — | — |
| T1036 | Masquerading | ..001 Invalid Code Signature; ..002 Right-to-Left Override; ..003 Rename Legitimate Utilities; ..004 Masquerade Task or Service; ..005 Match Legitimate Resource Name or Location; ..006 Space after Filename; ..007 Double File Extension; ..008 Masquerade File Type; ..009 Break Process Trees; ..010 Masquerade Account Name; ..011 Overwrite Process Arguments; ..012 Browser Fingerprint | — |
| T1027 | Obfuscated Files or Information | ..001 Binary Padding; ..002 Software Packing; ..003 Steganography; ..004 Compile After Delivery; ..005 Indicator Removal from Tools; ..006 HTML Smuggling; ..007 Dynamic API Resolution; ..008 Stripped Payloads; ..009 Embedded Payloads; ..010 Command Obfuscation; ..011 Fileless Storage; ..012 LNK Icon Smuggling; ..013 Encrypted/Encoded File; ..014 Polymorphic Code; ..015 Compression; ..016 Junk Code Insertion; ..017 SVG Smuggling; ..018 Invisible Unicode | — |
| T1620 | Reflective Code Loading | — | — |
| T1014 | Rootkit | — | — |
| T1679 | Selective Exclusion | — | — |
| T1684 | Social Engineering | ..001 Impersonation; ..002 Email Spoofing | — |
| T1218 | System Binary Proxy Execution | ..001 Compiled HTML File; ..002 Control Panel; ..003 CMSTP; ..004 InstallUtil; ..005 Mshta; ..007 Msiexec; ..008 Odbcconf; ..009 Regsvcs/Regasm; ..010 Regsvr32; ..011 Rundll32; ..012 Verclsid; ..013 Mavinject; ..014 MMC; ..015 Electron Applications | — |
| T1216 | System Script Proxy Execution | ..001 PubPrn; ..002 SyncAppvPublishingServer | — |
| T1221 | Template Injection | — | — |
| T1535 | Unused/Unsupported Cloud Regions | — | — |
| T1497 | Virtualization/Sandbox Evasion | ..001 System Checks; ..002 User Activity Based Checks; ..003 Time Based Checks | Discovery |
| T1220 | XSL Script Processing | — | — |

## TA0112 Defense Impairment

| ID | Техника | Подтехники | Также в тактиках |
| --- | --- | --- | --- |
| T1686 | Disable or Modify System Firewall | ..001 Cloud Firewall; ..002 Network Device Firewall; ..003 Windows Host Firewall | — |
| T1685 | Disable or Modify Tools | ..001 Disable or Modify Windows Event Log; ..002 Disable or Modify Cloud Log; ..003 Modify or Spoof Tool UI; ..004 Disable or Modify Linux Audit System Log; ..005 Clear Windows Event Logs; ..006 Clear Linux or Mac System Logs; ..007 Impair Autonomous Defenses | — |
| T1689 | Downgrade Attack | — | — |
| T1687 | Exploitation for Defense Impairment | — | — |
| T1222 | File and Directory Permissions Modification | ..001 Windows Permissions; ..002 Linux and Mac Permissions | — |
| T1578 | Modify Cloud Compute Infrastructure | ..001 Create Snapshot; ..002 Create Cloud Instance; ..003 Delete Cloud Instance; ..004 Revert Cloud Instance; ..005 Modify Cloud Compute Configurations | — |
| T1666 | Modify Cloud Resource Hierarchy | — | — |
| T1601 | Modify System Image | ..001 Patch System Image; ..002 Downgrade System Image | — |
| T1599 | Network Boundary Bridging | ..001 Network Address Translation Traversal | — |
| T1647 | Plist File Modification | — | — |
| T1690 | Prevent Command History Logging | — | — |
| T1207 | Rogue Domain Controller | — | — |
| T1688 | Safe Mode Boot | — | — |
| T1553 | Subvert Trust Controls | ..001 Gatekeeper Bypass; ..002 Code Signing; ..003 SIP and Trust Provider Hijacking; ..004 Install Root Certificate; ..005 Mark-of-the-Web Bypass; ..006 Code Signing Policy Modification | — |
| T1600 | Weaken Encryption | ..001 Reduce Key Space; ..002 Disable Crypto Hardware | — |

## TA0006 Credential Access

| ID | Техника | Подтехники | Также в тактиках |
| --- | --- | --- | --- |
| T1557 | Adversary-in-the-Middle | ..001 Name Resolution Poisoning and SMB Relay; ..002 ARP Cache Poisoning; ..003 DHCP Spoofing; ..004 Evil Twin | Collection |
| T1110 | Brute Force | ..001 Password Guessing; ..002 Password Cracking; ..003 Password Spraying; ..004 Credential Stuffing | — |
| T1555 | Credentials from Password Stores | ..001 Keychain; ..002 Securityd Memory; ..003 Credentials from Web Browsers; ..004 Windows Credential Manager; ..005 Password Managers; ..006 Cloud Secrets Management Stores | — |
| T1212 | Exploitation for Credential Access | — | — |
| T1187 | Forced Authentication | — | — |
| T1606 | Forge Web Credentials | ..001 Web Cookies; ..002 SAML Tokens | — |
| T1056 | Input Capture | ..001 Keylogging; ..002 GUI Input Capture; ..003 Web Portal Capture; ..004 Credential API Hooking | Collection |
| T1111 | Multi-Factor Authentication Interception | — | — |
| T1621 | Multi-Factor Authentication Request Generation | — | — |
| T1040 | Network Sniffing | — | Discovery |
| T1003 | OS Credential Dumping | ..001 LSASS Memory; ..002 Security Account Manager; ..003 NTDS; ..004 LSA Secrets; ..005 Cached Domain Credentials; ..006 DCSync; ..007 Proc Filesystem; ..008 /etc/passwd and /etc/shadow | — |
| T1528 | Steal Application Access Token | — | — |
| T1697 | Steal Application Specific Passwords | — | — |
| T1649 | Steal or Forge Authentication Certificates | — | — |
| T1558 | Steal or Forge Kerberos Tickets | ..001 Golden Ticket; ..002 Silver Ticket; ..003 Kerberoasting; ..004 AS-REP Roasting; ..005 Ccache Files | — |
| T1539 | Steal Web Session Cookie | — | — |
| T1552 | Unsecured Credentials | ..001 Credentials In Files; ..002 Credentials in Registry; ..003 Shell History; ..004 Private Keys; ..005 Cloud Instance Metadata API; ..006 Group Policy Preferences; ..007 Container API; ..008 Chat Messages | — |

## TA0007 Discovery

| ID | Техника | Подтехники | Также в тактиках |
| --- | --- | --- | --- |
| T1087 | Account Discovery | ..001 Local Account; ..002 Domain Account; ..003 Email Account; ..004 Cloud Account | — |
| T1010 | Application Window Discovery | — | — |
| T1217 | Browser Information Discovery | — | — |
| T1580 | Cloud Infrastructure Discovery | — | — |
| T1538 | Cloud Service Dashboard | — | — |
| T1526 | Cloud Service Discovery | — | — |
| T1619 | Cloud Storage Object Discovery | — | — |
| T1613 | Container and Resource Discovery | — | — |
| T1652 | Device Driver Discovery | — | — |
| T1482 | Domain Trust Discovery | — | — |
| T1083 | File and Directory Discovery | — | — |
| T1615 | Group Policy Discovery | — | — |
| T1680 | Local Storage Discovery | — | — |
| T1654 | Log Enumeration | — | — |
| T1046 | Network Service Discovery | — | — |
| T1135 | Network Share Discovery | — | — |
| T1201 | Password Policy Discovery | — | — |
| T1120 | Peripheral Device Discovery | — | — |
| T1069 | Permission Groups Discovery | ..001 Local Groups; ..002 Domain Groups; ..003 Cloud Groups | — |
| T1057 | Process Discovery | — | — |
| T1012 | Query Registry | — | — |
| T1018 | Remote System Discovery | — | — |
| T1518 | Software Discovery | ..001 Security Software Discovery; ..002 Backup Software Discovery | — |
| T1082 | System Information Discovery | — | — |
| T1614 | System Location Discovery | ..001 System Language Discovery | — |
| T1016 | System Network Configuration Discovery | ..001 Internet Connection Discovery; ..002 Wi-Fi Discovery | — |
| T1049 | System Network Connections Discovery | — | — |
| T1033 | System Owner/User Discovery | — | — |
| T1007 | System Service Discovery | — | — |
| T1124 | System Time Discovery | — | — |
| T1673 | Virtual Machine Discovery | — | — |

## TA0008 Lateral Movement

| ID | Техника | Подтехники | Также в тактиках |
| --- | --- | --- | --- |
| T1210 | Exploitation of Remote Services | — | — |
| T1534 | Internal Spearphishing | — | — |
| T1570 | Lateral Tool Transfer | — | — |
| T1563 | Remote Service Session Hijacking | ..001 SSH Hijacking; ..002 RDP Hijacking | — |
| T1021 | Remote Services | ..001 Remote Desktop Protocol; ..002 SMB/Windows Admin Shares; ..003 Distributed Component Object Model; ..004 SSH; ..005 VNC; ..006 Windows Remote Management; ..007 Cloud Services; ..008 Direct Cloud VM Connections | — |
| T1080 | Taint Shared Content | — | — |
| T1550 | Use Alternate Authentication Material | ..001 Application Access Token; ..002 Pass the Hash; ..003 Pass the Ticket; ..004 Web Session Cookie | — |

## TA0009 Collection

| ID | Техника | Подтехники | Также в тактиках |
| --- | --- | --- | --- |
| T1560 | Archive Collected Data | ..001 Archive via Utility; ..002 Archive via Library; ..003 Archive via Custom Method | — |
| T1123 | Audio Capture | — | — |
| T1119 | Automated Collection | — | — |
| T1185 | Browser Session Hijacking | — | — |
| T1115 | Clipboard Data | — | — |
| T1530 | Data from Cloud Storage | — | — |
| T1602 | Data from Configuration Repository | ..001 SNMP (MIB Dump); ..002 Network Device Configuration Dump | — |
| T1213 | Data from Information Repositories | ..001 Confluence; ..002 Sharepoint; ..003 Code Repositories; ..004 Customer Relationship Management Software; ..005 Messaging Applications; ..006 Databases | — |
| T1005 | Data from Local System | — | — |
| T1039 | Data from Network Shared Drive | — | — |
| T1025 | Data from Removable Media | — | — |
| T1074 | Data Staged | ..001 Local Data Staging; ..002 Remote Data Staging | — |
| T1114 | Email Collection | ..001 Local Email Collection; ..002 Remote Email Collection; ..003 Email Forwarding Rule | — |
| T1113 | Screen Capture | — | — |
| T1125 | Video Capture | — | — |

## TA0011 Command and Control

| ID | Техника | Подтехники | Также в тактиках |
| --- | --- | --- | --- |
| T1071 | Application Layer Protocol | ..001 Web Protocols; ..002 File Transfer Protocols; ..003 Mail Protocols; ..004 DNS; ..005 Publish/Subscribe Protocols | — |
| T1092 | Communication Through Removable Media | — | — |
| T1132 | Data Encoding | ..001 Standard Encoding; ..002 Non-Standard Encoding | — |
| T1001 | Data Obfuscation | ..001 Junk Data; ..002 Steganography; ..003 Protocol or Service Impersonation | — |
| T1568 | Dynamic Resolution | ..001 Fast Flux DNS; ..002 Domain Generation Algorithms; ..003 DNS Calculation | — |
| T1573 | Encrypted Channel | ..001 Symmetric Cryptography; ..002 Asymmetric Cryptography | — |
| T1008 | Fallback Channels | — | — |
| T1665 | Hide Infrastructure | — | — |
| T1105 | Ingress Tool Transfer | — | — |
| T1104 | Multi-Stage Channels | — | — |
| T1095 | Non-Application Layer Protocol | — | — |
| T1571 | Non-Standard Port | — | — |
| T1572 | Protocol Tunneling | — | — |
| T1090 | Proxy | ..001 Internal Proxy; ..002 External Proxy; ..003 Multi-hop Proxy; ..004 Domain Fronting | — |
| T1219 | Remote Access Tools | ..001 IDE Tunneling; ..002 Remote Desktop Software; ..003 Remote Access Hardware | — |
| T1102 | Web Service | ..001 Dead Drop Resolver; ..002 Bidirectional Communication; ..003 One-Way Communication | — |

## TA0010 Exfiltration

| ID | Техника | Подтехники | Также в тактиках |
| --- | --- | --- | --- |
| T1700 | AI-Enabled Exfiltration | — | — |
| T1020 | Automated Exfiltration | ..001 Traffic Duplication | — |
| T1030 | Data Transfer Size Limits | — | — |
| T1048 | Exfiltration Over Alternative Protocol | ..001 Exfiltration Over Symmetric Encrypted Non-C2 Protocol; ..002 Exfiltration Over Asymmetric Encrypted Non-C2 Protocol; ..003 Exfiltration Over Unencrypted Non-C2 Protocol | — |
| T1041 | Exfiltration Over C2 Channel | — | — |
| T1011 | Exfiltration Over Other Network Medium | ..001 Exfiltration Over Bluetooth | — |
| T1052 | Exfiltration Over Physical Medium | ..001 Exfiltration over USB | — |
| T1567 | Exfiltration Over Web Service | ..001 Exfiltration to Code Repository; ..002 Exfiltration to Cloud Storage; ..003 Exfiltration to Text Storage Sites; ..004 Exfiltration Over Webhook | — |
| T1029 | Scheduled Transfer | — | — |
| T1537 | Transfer Data to Cloud Account | — | — |

## TA0040 Impact

| ID | Техника | Подтехники | Также в тактиках |
| --- | --- | --- | --- |
| T1531 | Account Access Removal | — | — |
| T1485 | Data Destruction | ..001 Lifecycle-Triggered Deletion | — |
| T1486 | Data Encrypted for Impact | — | — |
| T1565 | Data Manipulation | ..001 Stored Data Manipulation; ..002 Transmitted Data Manipulation; ..003 Runtime Data Manipulation | — |
| T1491 | Defacement | ..001 Internal Defacement; ..002 External Defacement | — |
| T1561 | Disk Wipe | ..001 Disk Content Wipe; ..002 Disk Structure Wipe | — |
| T1667 | Email Bombing | — | — |
| T1499 | Endpoint Denial of Service | ..001 OS Exhaustion Flood; ..002 Service Exhaustion Flood; ..003 Application Exhaustion Flood; ..004 Application or System Exploitation | — |
| T1657 | Financial Theft | — | — |
| T1495 | Firmware Corruption | — | — |
| T1490 | Inhibit System Recovery | — | — |
| T1498 | Network Denial of Service | ..001 Direct Network Flood; ..002 Reflection Amplification | — |
| T1496 | Resource Hijacking | ..001 Compute Hijacking; ..002 Bandwidth Hijacking; ..003 SMS Pumping; ..004 Cloud Service Hijacking | — |
| T1489 | Service Stop | — | — |
| T1529 | System Shutdown/Reboot | — | — |

## Что делать с этим списком

- **Приоритизация.** Вычеркните техники, которые физически неприменимы в вашей среде (платформа, права, отсутствие сервиса). Остальное — рабочий список для покрытия.
- **Маппинг правил.** Тег в правиле SIEM/EDR должен указывать на подтехнику, а не на тактику: `T1059.001`, а не «Execution».
- **Покрытие.** По каждой технике из приоритетного списка нужен ответ: есть аналитика, есть источник данных (Data Component), проверено эмуляцией.
- **Версии.** При обновлении ATT&CK сверяйте ID: техники переезжают между родителями (в v20 `T1070.001` → `T1685.005`) и уходят в deprecated.

## Связанные заметки

- [[Techniques]] — как устроены техники и подтехники, анатомия страницы, связка с детектами
- [[Att&ck]] — обзор модели и 15 тактик Enterprise
- [[ЖИЗНЕННЫЙ ЦИКЛ АТАКИ (KILL CHAIN)]] — этапы атаки
- [[Инциденты ИБ]] — разбор инцидентов в терминах техник
- [[Defense in Depth]] — проверка покрытия слоёв защиты
- [[Pentest]] — практическая проверка защищённости

## Источники

- Матрица Enterprise: https://attack.mitre.org/matrices/enterprise/
- Список техник Enterprise: https://attack.mitre.org/techniques/enterprise/
- Release notes v20 (октябрь 2026): https://attack.mitre.org/resources/updates/updates-october-2026
- ATT&CK Navigator: https://mitre-attack.github.io/attack-navigator/
