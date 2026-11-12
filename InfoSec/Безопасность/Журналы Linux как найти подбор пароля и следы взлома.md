
**Смысл:** любой сервер ведёт журнал — кто пытался войти, кто вошёл, кто стал администратором и что делал. Заметка учит читать эти записи и отличать шум от реального взлома.

## 1. Зачем читать журналы

Журнал отвечает на три вопроса: 

1. **была ли атака**
2. **удалась ли она** 
3. **что делали после входа**. 

Без логов инцидент невозможно восстановить: нельзя понять, подобрали пароль или нет, и какие действия успел совершить злоумышленник.

## 2. Где лежат журналы

- **Текстовые логи:** `/var/log/auth.log` (Debian/Ubuntu), `/var/log/secure` (RHEL/Fedora/CentOS) — сюда пишет SSH и sudo.
- **systemd-journald:** бинарный журнал, читается через `journalctl` (у него своё хранилище, а не файл в `/var/log`).
- Дополнительно: `/var/log/syslog`, `/var/log/messages`, `/var/log/audit/audit.log`.
- Читать большинство логов нужно через `sudo`.

```
sudo tail -f /var/log/auth.log
journalctl -u ssh            # Red Hat / Fedora: -u sshd
journalctl --since yesterday
journalctl -f                # следить в реальном времени
```

## 3. Три типа строк о входе по SSH

1. **`Failed password for …`** — неудачная попытка входа (пароль не подошёл).
2. **`Invalid user …`** — попытка войти под **несуществующим** пользователем. Почти всегда признак перебора или сканера.
3. **`Accepted password / publickey for …`** — **успешный** вход (по паролю или по ключу).

Вокруг них полезны и сопутствующие строки: `Connection closed by authenticating user`, `session opened for user`, `Disconnected from invalid user`.

## 4. Как понять, кто подбирает пароль

```
sudo grep 'Failed password' /var/log/auth.log \
  | awk '{print $(NF-3)}' | sort | uniq -c | sort -rn | head
```

Как читать конвейер:

- `grep 'Failed password'` — оставить только неудачные попытки;
- `awk '{print $(NF-3)}'` — вытащить IP (в строке вида `Failed password for root from 1.2.3.4 port 22 ssh2` адрес стоит за три поля до конца);
- `sort | uniq -c` — посчитать повторы;
- `sort -rn | head` — показать топ источников.

То же по несуществующим пользователям и по времени:

```
sudo grep 'Invalid user' /var/log/auth.log | awk '{print $(NF-5)}' | sort | uniq -c | sort -rn | head
journalctl -u ssh --since "1 hour ago" | grep -c 'Failed password'
sudo lastb -n 20
```

Смысл: за минуту получить список IP, которые "долбят" сервер, и понять масштаб.

## 5. Главное правило и четыре сигнала взлома

**Главное правило:** смотреть не на одну строку, а на **последовательность событий** — и делать это регулярно, пока журнал не перезаписан. Отсутствие записей там, где они должны быть, — тоже тревожный сигнал.

**Четыре сигнала, которые разбираются :**

1. **Массовые `Failed password` / `Invalid user`** с одного адреса — идёт подбор.
2. **`Accepted` после серии неудач**, с незнакомого IP или в необычное время — подбор, похоже, удался.
3. **Новый пользователь или изменение прав sudo** — попытка закрепиться в системе.
4. **Новый SSH-ключ или новая задача в расписании** (cron/systemd) — скрытый канал доступа и автозапуск.

## 6. Ложные тревоги

Не всё, что выглядит как атака, — атака:

- собственные входы администратора и коллег;
- интернет-сканеры и боты, которые круглосуточно стучатся в порт 22 — это фон, а не взлом;
- автоматика: мониторинг, бэкапы, CI/CD, cron-задачи, сервисные учётки;
- NAT/корпоративный прокси: за одним внешним IP может стоять много людей;
- перезапуски сервисов и ротация логов.

Поэтому вывод делается по совокупности: источник, частота, время, успешность, дальнейшие действия.

## 7. `last`, `lastb`, `who`

```
last -n 3          # последние успешные входы (читает wtmp)
sudo lastb -n 2    # последние неудачные входы (btmp, нужен root)
who                # кто сейчас в системе
w                  # кто сейчас и что делает
```

`last` показывает также перезагрузки и сессии — удобно отвечать на вопрос «кто и когда входил».

## 8. Следы после входа (4:38)

Злоумышленник после успешного входа обычно оставляет следы в четырёх местах — их и надо проверять.

**Пользователи**

```
sudo grep 'new user' /var/log/auth.log
```

Плюс глазами: `/etc/passwd` (нет ли лишних учёток и UID 0), группы `sudo`/`wheel`, `sudo -l`.

**Права sudo**

```
journalctl _COMM=sudo --since today
sudo grep 'sudo:' /var/log/auth.log
```

Проверить `/etc/sudoers` и `/etc/sudoers.d` — не появилось ли правило без пароля.

**SSH-ключи**

```
sudo auditctl -w /root/.ssh/authorized_keys -p wa -k sshkeys
sudo ausearch -k sshkeys
```

`auditctl` ставит наблюдение за записью/изменением атрибутов (wa), `ausearch` показывает события по метке. Дополнительно просмотреть `~/.ssh/authorized_keys` у всех пользователей.

**Расписание и автозапуск**

```
journalctl -u cron                 # Red Hat: -u crond
sudo crontab -l -u ИМЯ
systemctl list-unit-files --state=enabled # автозагрузка программ
```

Плюс `/etc/cron.*`, systemd-таймеры — типовое место для закрепления.

## 9. Ключи `journalctl` и хранение журнала

```
journalctl -u ssh
journalctl -u ssh --since "1 hour ago"
journalctl --since yesterday --until "10:00"
journalctl -f
journalctl -n 50
journalctl -p err
journalctl -b                 # только текущая загрузка
journalctl _COMM=sudo --since today
```

Важный момент: по умолчанию journald может хранить журнал **в памяти** (`/run`), и после перезагрузки он пропадёт. Чтобы журнал выжил:

```
# /etc/systemd/journald.conf
[Journal]
Storage=persistent
```

```
sudo systemctl restart systemd-journald
```

Плюс ограничения размера и срока: `SystemMaxUse`, `MaxRetentionSec`; проверить занятое место — `journalctl --disk-usage`, почистить — `--vacuum-time=30d`.

## 10. Защита самих журналов

Атакующий первым делом пытается **замести следы**: удалить или обрезать логи, отключить rsyslog, очистить journal. Поэтому журнал — тоже объект защиты:

- **Права:** логи принадлежат `root`, группа `adm`, режим 640; каталоги — 750.
- **Пересылка на другой хост:** rsyslog (`*.* @@logserver:514`) или `systemd-journal-remote` — тогда локальная чистка не уничтожит копию.
- **Append-only:** `sudo chattr +a /var/log/auth.log` (учитывать взаимодействие с logrotate).
- **Отдельный диск/раздел** под логи, чтобы их нельзя было «забить» и чтобы они пережили проблему с корнем.
- **auditd** — наблюдение за критичными файлами (ключи, sudoers, cron).
- **Мониторинг и алерты:** fail2ban, SIEM/SOC, уведомления о всплесках неудачных входов.

## 11. Итог

1. Проверить, **где** лежат журналы и **ведётся** ли journald постоянно.
2. Найти **всплеск** неудачных входов и посчитать источники.
3. Проверить **успешные** входы: не было ли `Accepted` после серии `Failed password`.
4. Искать **закрепление**: пользователи, sudo, ключи, cron/systemd.
5. **Защитить логи** (права, пересылка, append-only) и повторять проверку регулярно.

---

## Шпаргалка

|Задача|Команда|
|---|---|
|Топ IP по подбору|`grep 'Failed password' /var/log/auth.log \| awk '{print $(NF-3)}' \| sort \| uniq -c \| sort -rn \| head`|
|Лог SSH|`journalctl -u ssh` (RHEL: `-u sshd`)|
|За последний час|`journalctl -u ssh --since "1 hour ago"`|
|С вчерашнего дня|`journalctl --since yesterday`|
|Следить онлайн|`journalctl -f`|
|Действия sudo|`journalctl _COMM=sudo --since today`|
|Кто входил / неудачи / кто сейчас|`last -n 3` · `sudo lastb -n 2` · `who` · `w`|
|Новые пользователи|`sudo grep 'new user' /var/log/auth.log`|
|Контроль ключей|`sudo auditctl -w /root/.ssh/authorized_keys -p wa -k sshkeys` → `sudo ausearch -k sshkeys`|
|Расписание|`journalctl -u cron` (RHEL: `crond`) · `sudo crontab -l -u ИМЯ` · `systemctl list-unit-files --state=enabled`|
|Постоянное хранение|`/etc/systemd/journald.conf` → `Storage=persistent` → `sudo systemctl restart systemd-journald`|
|Пересылка логов|rsyslog, `systemd-journal-remote`|

**Ключевая мысль:** журнал — это доказательная база. Ценность не в одной команде `grep`, а в привычке регулярно смотреть цепочку «попытка → вход → закрепление» и в том, чтобы логи было невозможно тихо переписать.
