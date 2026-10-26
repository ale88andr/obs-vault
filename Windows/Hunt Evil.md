---
title: Hunt Evil — Lateral Movement
tags:
  - windows
  - dfir
  - threat-hunting
  - lateral-movement
  - sans
source: Windows/Hunt Evil.pdf
---

# Hunt Evil: Lateral Movement (SANS FOR508)

Конспект постера **SANS «Hunt Evil: Lateral Movement»** (FOR508, dfir.sans.org, версия от января 2021). Файл в хранилище называется [[Hunt Evil.pdf]], но по содержанию это именно постер про боковое перемещение в Windows.

Идея постера: для каждой техники lateral movement показано несколько срезов — **что запускает атакующий** (командная строка), **следы в журналах событий** (Event Logs), **артефакты в системе** (реестр, файлы, Prefetch/Amcache/ShimCache) и **что именно анализировать** (Analysis). Ниже — сводка этих срезов.

## Матрица техник

| Техника | Инструменты (что искать в памяти/на диске) | Ключевые артефакты | Event ID |
| --- | --- | --- | --- |
| RDP | `mstsc.exe`, `rdpclip.exe`, `tstheme.exe` | Terminal Server Client\Servers, AutomaticDestinations, bitmap cache, Prefetch | 4624 (type 10), 4648, 1149, 1024, 1102 |
| net use / админ-шары | `net.exe`, `net1.exe` | MountPoints2, Shellbags, SmbClient/Security | 4624 (type 3), 4648, 1102 |
| PsExec | `psexec.exe`, `psexesvc.exe`, `evil.exe` | служба PSEXESVC, ShimCache, Amcache, Prefetch | 7045, 4624 (type 2/3), 4648 |
| Планировщик задач | `at.exe`, `schtasks.exe` | C:\Windows\System32\Tasks, TaskCache\Tasks и \Tree | 4698, 4699, 106, 140, 141 |
| Создание службы | `sc.exe` | ветка `CurrentControlSet\Services\*`, ShimCache, Amcache | 7045, 7034, 7035, 7036, 7040 |
| WMI / WMIC | `wmic.exe`, `wmiprvse.exe`, `scrcons.exe`, `mofcomp.exe` | WMI-Activity, репозиторий WMI, Prefetch | 5857–5861 (5860), 40961/40962, 4624, 4648 |
| PowerShell Remoting / WinRM | `powershell.exe`, `wsmprovhost.exe` | ConsoleHost_history.txt, ExecutionPolicy, Prefetch | 4103, 4104, 40961/40962, 53504 |

## RDP

Запуск сессии с рабочей станции атакующего:

```cmd
mstsc.exe /v:host
```

Куда смотреть:

- **Журналы:** `Microsoft-Windows-TerminalServices-RDPClient/Operational.evtx`, `Microsoft-Windows-RdpCoreTS/Operational.evtx`, `RemoteDesktopServices-*`, Security.evtx (4624 type 10 — вход по RDP, 4648 — явное использование учётных данных, 1149 — успешная аутентификация RDP, 1024/1102 — сбой/очистка журнала).
- **Реестр:** `NTUSER.DAT\Software\Microsoft\Terminal Server Client\Servers` — список хостов, к которым подключались, с именами пользователей.
- **Файлы:** `%APPDATA%\Microsoft\Windows\Recent\AutomaticDestinations\{MSTSC-APPID}-automaticDestinations-ms` — jump list с историей подключений (имя хоста, время).
- **Кэш RDP:** `%LOCALAPPDATA%\Microsoft\Terminal Server Client\Cache` — `bcache##.bmc`, `cache####.bin` (фрагменты экрана удалённой сессии).
- **Прочее:** Prefetch `mstsc.exe-{hash}.pf`, `rdpclip.exe-{hash}.pf`, `tstheme.exe-{hash}.pf`, записи в ShimCache и Amcache.

## net use и доступ к административным шарам

```cmd
net use z: \\host\c$ /user:domain\username <password>
```

На что обращать внимание:

- **Процессы и Prefetch:** `net.exe`, `net1.exe` и их `*-{hash}.pf`. `net1.exe` — признак того, что `net.exe` действительно отработал, а не был просто вызван.
- **Реестр:** `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\MountPoints2` — следы подключённых сетевых ресурсов.
- **Shellbags:** `USRCLASS.DAT` — факт просмотра содержимого удалённых каталогов даже после отключения.
- **Журналы:** `Microsoft-Windows-SmbClient/Security.evtx`, Security.evtx — 4624 type 3 (сетевой вход), 4648, 1102.
- **Анализ:** отличать легитимную работу администраторов/скриптов от ручного перебора шар `C$` и `ADMIN$`.

## PsExec

```cmd
psexec.exe \\host -accepteula -d -c c:\temp\evil.exe
```

Индикаторы:

- **Файловая система:** `psexec.exe`, `psexesvc.exe`, доставленный `evil.exe` (+ Prefetch `*-{hash}.pf`).
- **Реестр:** `...\PsExec\EulaAccepted` в NTUSER.DAT — подтверждение запуска PsExec; служба `PSEXESVC` в `CurrentControlSet\Services`.
- **Журналы:** System.evtx — 7045 (установлена служба), Security.evtx — 4624 type 2/3 (вход на хост), 4648.
- **Прочее:** ShimCache и Amcache подтверждают факт запуска бинарника, даже если он уже удалён.

## Планировщик задач (schtasks / at)

```cmd
schtasks /CREATE /TN taskname /TR c:\temp\evil.exe /SC once /RU "SYSTEM" /ST 13:00 /S host /U username
at \\host 13:00 "c:\temp\evil.exe"
```

Следы:

- **Журналы:** Security.evtx — 4698 (задача создана), 4699 (удалена); `Microsoft-Windows-TaskScheduler/Operational.evtx` — 106 (задача зарегистрирована), 140/141 (обновление/удаление).
- **Файлы:** `C:\Windows\System32\Tasks\<task>` (XML самой задачи), `C:\Windows\Tasks` (для `at`).
- **Реестр:** `SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tasks` и `...\TaskCache\Tree` — кэш задач, сохраняется после удаления файла задачи.
- **Прочее:** Prefetch `schtasks.exe-{hash}.pf`, `at.exe-{hash}.pf`, записи в ShimCache/Amcache.

## Создание службы через sc.exe

```cmd
sc \\host create servicename binpath= "c:\temp\evil.exe"
sc \\host start servicename
```

Что проверять:

- **Журналы:** System.evtx — 7045 (новая служба), 7034 (неожиданное завершение), 7035 (отправлен запрос на управление), 7036 (изменение состояния), 7040 (изменён тип запуска).
- **Реестр:** `SYSTEM\CurrentControlSet\Services\<servicename>` — `ImagePath`, `Start`, `ServiceDll`.
- **Файлы:** `sc.exe-{hash}.pf`, `evil.exe-{hash}.pf`, ShimCache/Amcache по `sc.exe` и по самой службе.
- **Анализ:** служба с `ImagePath`, указывающим на `c:\temp` или `%TEMP%`, — классический признак закрепления через службу.

## WMI / WMIC

```cmd
wmic /node:host process call create "C:\temp\evil.exe"
```

```powershell
Invoke-WmiMethod -Computer host -Class Win32_Process -Name create -Argument "c:\temp\evil.exe"
```

Индикаторы:

- **Процессы:** `wmic.exe`, `wmiprvse.exe` (хост-процесс WMI), `scrcons.exe` (постоянные подписки на события), `mofcomp.exe` (компиляция `.mof`).
- **Журналы:** `Microsoft-Windows-WMI-Activity/Operational.evtx` — 5857–5861 (в т.ч. 5860 — операция с классом/методом), 40961/40962; Security.evtx — 4624 type 3, 4648.
- **Файлы:** `C:\Windows\System32\wbem\Repository` — постоянно закреплённые подписки WMI сохраняются здесь; `*-{hash}.pf` для `wmic.exe`, `wmiprvse.exe`, `scrcons.exe`, `mofcomp.exe`.
- **Анализ:** связка «WMI-запрос → запуск процесса на удалённом хосте» хорошо видна по времени между 5860 и 4624/4648.

## PowerShell Remoting и WinRM

```powershell
Enter-PSSession –ComputerName host
Invoke-Command –ComputerName host –ScriptBlock {Start-Process c:\temp\evil.exe}
```

Где искать:

- **Журналы:** `Microsoft-Windows-PowerShell/Operational.evtx` — 4103 (Module logging), 4104 (Script Block logging, видно сам код), 40961/40962 (запуск PowerShell-движка), 800/403/168; `Microsoft-Windows-WinRM/Operational.evtx` — 91, 168, 194, 53504 (инициализация сессии WinRM/WSMan), 40961/40962.
- **Файлы:** `C:\Users\<username>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt` — история интерактивных команд по пользователю.
- **Реестр:** `...\ShellIds\Microsoft.PowerShell\ExecutionPolicy` — смена политики выполнения.
- **Процессы:** `powershell.exe`, `wsmprovhost.exe` (хост WinRM), Prefetch для обоих.
- **Анализ:** удалённый запуск через WinRM почти всегда оставляет пару «53504 на хосте-цели + 4104 с подозрительным ScriptBlock».

## Артефакты выполнения программ (нижний блок постера)

Отдельная таблица «Evidence of Program Execution» — универсальные артефакты, которые подтверждают запуск приложения даже после удаления файла.

| Артефакт | Что показывает | Расположение |
| --- | --- | --- |
| UserAssist | Запущенные через Explorer программы, счётчик запусков и время | `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{GUID}\Count` |
| RecentApps | Недавно запускавшиеся приложения (Win10) | `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Search\RecentApps` |
| ShimCache (AppCompatCache) | Факт присутствия/запуска исполняемого файла, путь и время | `SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache` |
| Jump Lists | Недавние документы/хосты по приложению | `C:\%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations` |
| Prefetch | Запуск программы: путь, время, загруженные DLL (до 1024 записей) | `C:\Windows\Prefetch\*.pf` |
| Amcache | Метаданные исполняемого файла, хэш, AppID | `Amcache.hve\Root\File\{Volume GUID}\#######` |

Пояснения из постера:

- UserAssist хранит данные в закодированном виде (ROT13 по имени значения) и привязан к GUID сессии/ОС (для Win7 и Win10 GUID разные).
- ShimCache не содержит времени запуска в старых версиях Windows — только факт наличия файла; на Win10 есть отметка времени последнего изменения.
- Prefetch можно отключить, но на рабочих станциях он обычно включён и содержит до 1024 записей.
- Amcache привязан к тому GUID тома, с которого запускался файл; содержит имя приложения и хэш.

## Быстрый справочник Event ID

| ID | Журнал | Смысл |
| --- | --- | --- |
| 4624 | Security | Успешный вход (type 2 — локально, 3 — по сети, 10 — RDP) |
| 4648 | Security | Вход с явно указанными учётными данными |
| 4698 / 4699 | Security | Задача создана / удалена |
| 7045 | System | Установлена новая служба |
| 7034 / 7035 / 7036 / 7040 | System | Сбой службы / управляющий запрос / смена состояния / смена типа запуска |
| 106 / 140 / 141 | TaskScheduler/Operational | Задача зарегистрирована / обновлена / удалена |
| 1149 | TerminalServices | Успешная RDP-аутентификация |
| 5860 (5857–5861) | WMI-Activity/Operational | Операции WMI (в т.ч. удалённый вызов метода) |
| 4103 / 4104 | PowerShell/Operational | Module logging / Script Block logging |
| 40961 / 40962 | PowerShell/Operational | Запуск PowerShell-движка |
| 53504 | WinRM/Operational | Инициализация сессии WinRM/WSMan |

## Практические выводы

- Одна и та же техника оставляет след сразу в трёх плоскостях: **журнал события → артефакт в реестре/ФС → исполняемый файл в Prefetch/Amcache/ShimCache**. Самая надёжная атрибуция получается при совпадении времени во всех трёх.
- Удаление инструмента (`psexec.exe`, `schtasks.exe`, `evil.exe`) не уничтожает улики: Prefetch, ShimCache, Amcache, TaskCache и WMI-репозиторий остаются.
- Командные строки на постере — готовые шаблоны для проверки гипотез: их можно воспроизвести в тестовом стенде и посмотреть, какие именно события появятся.
- Полезно держать под рукой связку «техника → Event ID → артефакт» из таблиц выше при разборе инцидента и при написании правил детекта.

## Ссылки

- SANS FOR508 — Advanced Incident Response, Threat Hunting, and Digital Forensics: <https://www.sans.org/cyber-security-courses/advanced-incident-response-threat-hunting-training/>
- DFIR-ресурсы SANS: <https://dfir.sans.org>
