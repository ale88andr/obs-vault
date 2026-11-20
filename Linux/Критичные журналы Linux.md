
**Идея:** журналы — это «тетради» на проходной сервера. Чтобы быстро понять, что случилось, надо знать, **в какую тетрадь смотреть** и **какие строки читать первыми**. Вся тема укладывается в шесть вопросов: **Who, What, When, Where, Why, How**.

## 0. Проходная и четыре тетради

Метафора: на проходной лежат журналы — кто вошёл, что делал, когда, где, почему и как. В Linux это те же «тетради», только разложены по `/var/log` и бинарному журналу systemd. Задача заметки — дать карту: какой вопрос закрывает какой журнал.

## 1. Две службы: journald и rsyslog

- **systemd-journald** — собирает логи от systemd, ядра и сервисов в **бинарный** журнал; читается через `journalctl`.
- **rsyslog** — классический демон, пишет **текстовые** файлы в `/var/log` по правилам `/etc/rsyslog.conf` и `/etc/rsyslog.d/`.
- Они работают вместе: journald может пересылать сообщения в rsyslog (и наоборот), поэтому одни и те же события можно найти и в `journalctl`, и в файле.

Практический вывод: `journalctl` удобен фильтрами, текстовые файлы — привычны и легко грепаются.

## 2. Debian и Red Hat: разные имена

| Вопрос     | Debian / Ubuntu                        | Red Hat / Rocky / Alma               |
| ---------- | -------------------------------------- | ------------------------------------ |
| **Кто**    | `/var/log/auth.log`                    | `/var/log/secure`                    |
| **Что**    | `/var/log/syslog`                      | `/var/log/messages`                  |
| **Когда**  | строки `CRON` в syslog                 | `/var/log/cron`                      |
| **Где**    | `/var/log/nginx/`, `/var/log/apache2/` | `/var/log/nginx/`, `/var/log/httpd/` |
| **Почему** | `/var/log/kern.log`, `dmesg`           | `dmesg`                              |
| **Как**    | `/var/log/audit/audit.log`             | `/var/log/audit/audit.log`           |

Одна и та же строка SSH лежит в `auth.log` или `secure` — первое, что нужно проверить на незнакомом сервере.

## 3. Ловушка: журнал только в памяти

По умолчанию journald может держать журнал в **`/run/log/journal`** (tmpfs) — после перезагрузки он **исчезает**. Это частая причина «логов нет, значит ничего не было».

Проверка и исправление:

```
journalctl --disk-usage
ls /var/log/journal            # если каталога нет — журнал в памяти
sudo mkdir -p /var/log/journal
sudo systemd-tmpfiles --create --prefix /var/log/journal
```

```
# /etc/systemd/journald.conf
[Journal]
Storage=persistent
```

```
sudo systemctl restart systemd-journald
```

После этого журнал переживает ребут. Полезно также задать `SystemMaxUse` и срок хранения.

## 4. Шесть вопросов

Каркас для триажа:

- **Who** — кто входил и повышал права;
- **What** — что происходило в системе;
- **When** — когда запускались задачи по расписанию;
- **Where** — где (какой сервис/сайт) происходили события;
- **Why** — почему система вела себя так (ядро, железо, OOM);
- **How** — как именно действовали (аудит вызовов и файлов).

Дальше — по одному журналу на каждый вопрос.

## 5. Who: журнал входов

`/var/log/auth.log` (Debian) или `/var/log/secure` (RHEL). Ключевые строки:

```
Failed password for invalid user admin from 203.0.113.5 port 44231 ssh2
Invalid user test from 203.0.113.5 port 44232
Accepted publickey for vasya from 192.168.1.20 port 55122 ssh2
session opened for user root by vasya(uid=1000)
new user: name=backdoor, UID=1001, GID=1001
```

Что искать: **`Failed password`**, **`Invalid user`**, **`Accepted`**, **`new user`**, **`session opened/closed`**, `su`, `authentication failure`.

```
sudo grep -E 'Failed password|Invalid user|Accepted' /var/log/auth.log
sudo grep 'new user' /var/log/auth.log
```

## 6. Как читать строку sudo

Типовая запись:

```
sudo:   vasya : TTY=pts/0 ; PWD=/home/vasya ; USER=root ; COMMAND=/usr/bin/apt install nmap
```

Разбор: кто (`vasya`), откуда (`TTY=pts/0`), где находился (`PWD`), от чьего имени выполнил (`USER=root`), что именно (`COMMAND`).

```
sudo grep 'sudo:' /var/log/auth.log
journalctl _COMM=sudo --since today
```

Аномалии: `COMMAND=/bin/bash`, `USER=root` из неожиданного TTY, `sudo` в нерабочее время.

## 7. wtmp, btmp, lastlog

Это **бинарные** файлы, читать их нужно утилитами:

```
last -n 10          # успешные входы (/var/log/wtmp)
sudo lastb -n 10    # неудачные входы (/var/log/btmp)
lastlog             # последний вход каждого пользователя
who                 # кто сейчас в системе
w                   # кто и что делает
```

`last` показывает также перезагрузки и сессии — удобно отвечать «кто входил и когда». `lastlog` быстро выявляет учётки, под которыми давно не входили (или наоборот — входили неожиданно).

## 8. What: syslog и messages

`/var/log/syslog` (Debian) / `/var/log/messages` (RHEL) — общий поток событий: старт/остановка сервисов, ошибки, сетевые сообщения, ядро. Это «журнал общего назначения», с которого начинают, когда непонятно, где искать.

```
sudo tail -f /var/log/syslog
journalctl --since "1 hour ago" -p warning
```

## 9. Журналы установки пакетов

Позволяют заметить **несанкционированную установку** ПО (часто первый шаг закрепления).

```
# Debian/Ubuntu
less /var/log/dpkg.log
less /var/log/apt/history.log
zgrep ' install ' /var/log/dpkg.log*.gz

# RHEL
less /var/log/dnf.log
less /var/log/yum.log
```

Смотреть: что установлено, когда, кем инициировано, не появилось ли инструментов для удалённого доступа/майнинга/скрытия следов.

## 10. When: cron

Задачи по расписанию — любимое место закрепления.

```
journalctl -u cron            # Debian
journalctl -u crond           # RHEL
grep CRON /var/log/syslog     # Debian
less /var/log/cron            # RHEL
crontab -l                    # свои задачи
sudo crontab -l -u ИМЯ        # чужие
ls -la /etc/cron.* /etc/crontab
systemctl list-timers --all   # systemd-таймеры
```

Искать: новые задания, запуск скриптов из `/tmp`, `/dev/shm`, скачивание по `curl|wget`, обратные подключения.

## 11. Where: веб-сервер, экран, fail2ban

- **Веб-сервер:** `/var/log/nginx/access.log`, `error.log`; `/var/log/apache2/` или `/var/log/httpd/`. В `access.log` — кто, откуда, какой URL, статус, User-Agent (сканеры, эксплуатация, web-shell).
- **Экран/графическая сессия:** журналы дисплей-менеджера и X/Wayland (`/var/log/Xorg.0.log`, journalctl юнита `gdm`/`lightdm`) — входы в графику и ошибки сессии.
- **fail2ban:** `/var/log/fail2ban.log` — кого банил и за что (косвенно показывает атаку).

```
sudo tail -f /var/log/nginx/access.log
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -rn | head
sudo grep 'Ban ' /var/log/fail2ban.log
```

## 12. Why: журнал ядра

`/var/log/kern.log` (Debian), `dmesg`, `journalctl -k`.

```
dmesg -T | tail -50
journalctl -k --since "1 hour ago"
```

Что искать: **OOM killer** (кого убили и почему), ошибки диска/ФС, сбои железа, USB-устройства, сообщения netfilter/iptables, паники. Это ответ на «почему сервер вёл себя странно».

## 13. How: журнал аудита

`auditd` пишет `/var/log/audit/audit.log` — самые детальные события: системные вызовы, доступ к файлам, запуск процессов.

```
sudo ausearch -m EXECVE -ts today
sudo ausearch -k sshkeys
sudo aureport --summary
```

Настройка наблюдения:

```
sudo auditctl -w /etc/passwd -p wa -k passwd_changes
sudo auditctl -w /root/.ssh/authorized_keys -p wa -k sshkeys
```

Это уровень «как именно»: какой процесс, с какими аргументами, кто и когда.

## 14. Пять минут на подозрение

Быстрый чек-лист при инциденте:

1. `last` / `lastb` — кто входил, были ли неудачи.
2. `grep -E 'Failed|Accepted|Invalid' auth.log` — брутфорс и успешные входы.
3. `grep 'new user'`, проверить `/etc/passwd`, группы sudo.
4. `journalctl _COMM=sudo` — повышение прав.
5. `crontab`, `/etc/cron.*`, таймеры — закрепление.
6. `dpkg.log`/`dnf.log` — новые пакеты.
7. Веб-логи и `fail2ban.log` — внешняя активность.

## 15. Четыре приёма работы с журналами

1. **Поиск:** `grep`, `zgrep` (для архивов `.gz`), `journalctl -g` (grep по журналу).
2. **Наблюдение:** `tail -f`, `journalctl -f` — смотреть в реальном времени.
3. **Фильтрация:** по времени (`--since/--until`), юниту (`-u`), приоритету (`-p`), процессу (`_COMM=`), загрузке (`-b`).
4. **Агрегация:** конвейеры `awk | sort | uniq -c | sort -rn` — топ IP, топ ошибок, частота событий.

Плюс работа с **ротацией**: `logrotate` переименовывает и сжимает логи — старые искать в `*.gz` через `zgrep`, иначе «в журнале пусто» после ротации.

## 16. Защита самих журналов

Атакующий первым делом чистит следы, поэтому журналы — объект защиты:

- **Права:** `root:adm`, 640 на файлы, 750 на каталоги.
- **Пересылка на другой хост:** rsyslog (`*.* @@logserver:514`) или `systemd-journal-remote` — локальная чистка не уничтожит копию.
- **Append-only:** `sudo chattr +a /var/log/auth.log` (учитывать `logrotate`).
- **Отдельный раздел** под `/var/log`, чтобы логи не забили корень и пережили сбой.
- **auditd** для ключевых файлов (passwd, sudoers, authorized_keys, cron).
- **Мониторинг/алерты:** fail2ban, SIEM, уведомления о всплесках неудачных входов.
- **Ротация и хранение:** достаточный срок, чтобы успеть расследовать инцидент.

## 17. Итог

- Журналов много, но они раскладываются по шести вопросам: **Who → auth.log/secure, What → syslog/messages, When → cron, Where → веб-логи, Why → kern.log/dmesg, How → audit.log**.
- Начинать расследование — с `auth.log`/`secure` и `last/lastb`, затем смотреть закрепление (пользователи, sudo, cron, пакеты) и только потом — глубокий аудит.
- Проверить, что журнал **вообще хранится** (`Storage=persistent`), иначе после ребута расследовать нечего.
- Защищать логи не менее важно, чем читать их.

---

## Шпаргалка «где что лежит»

|Вопрос|Debian/Ubuntu|Red Hat/Rocky/Alma|Команда|
|---|---|---|---|
|Who|`/var/log/auth.log`|`/var/log/secure`|`last`, `lastb`, `lastlog`|
|What|`/var/log/syslog`|`/var/log/messages`|`tail -f`, `journalctl`|
|When|`CRON` в syslog|`/var/log/cron`|`journalctl -u cron`|
|Where|`/var/log/nginx/`, `apache2/`|`/var/log/nginx/`, `httpd/`|`awk`, `grep` по access.log|
|Why|`/var/log/kern.log`, `dmesg`|`dmesg`|`journalctl -k`|
|How|`/var/log/audit/audit.log`|`/var/log/audit/audit.log`|`ausearch`, `aureport`|

**Главная мысль:** не нужно помнить десятки путей — достаточно держать в голове шесть вопросов и знать, какая «тетрадь» отвечает на каждый.