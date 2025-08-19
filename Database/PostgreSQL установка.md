
До загрузки PostgreSQL обновляем списки пакетов:

```bash
$ sudo apt update
```

Загрузим PostgreSQL с утилитой **-contrib**:

```bash
$ sudo apt install postgresql postgresql-contrib
```

Загрузятся драйверы PostgreSQL последней версии и развернутся необходимые компоненты на виртуальной машине с Ubuntu.

Запускаем сервис:

```bash
$ sudo systemctl start postgresql.service
```

Проверка статуса сервиса:

```bash
$ sudo systemctl status postgresql.service
```

## Работа с аккаунтом PostgreSQL

PostgreSQL применяет термин «Роль». Практически это тот же аккаунт в Ubuntu. При запуске СУБД роли сервиса привязываются к одноименным аккаунтам в Unix-системах. Другими словами, при наличии роли в PostgreSQL, войти в СУБД можно с аккаунтом Ubuntu. При запуске СУБД генерируется аккаунт `postgres`, привязываемый к роли PostgreSQL.

Войдем в аккаунт:

```
$ sudo -i -u postgres
```

После ввода команды видим подтверждение о переходе в аккаунт:

```
postgres@postgresdoc:~$
```

Откроем консоль Postgres:

```
$ psql
```

Консоль открыта, что подтверждается записью в начале строки:

```
postgres=#
```

Работа в СУБД ведется из консоли.

Узнать статус подключения:

```
postgres=# \conninfo
```

Возврат в аккаунт:

```
postgres=# \q
```

### Создание роли

Аккаунт `postgres` обладает правами администратора. Напишем `createuser`, эта команда сообщает, что мы добавляем новую роль. Чтобы указать имя роли и выдать суперюзера, применим флаг `--interactive`:

```
postgres@postgresdoc:~$ createuser --interactive
```

или вариант работы без переходов между аккаунтами:

```
$ sudo -u postgres createuser --interactive
```

Вводим имя, выдаем суперюзера:

```
Enter name of role to add: tester Shall the new role be a superuser? (y/n) y
```

Посмотреть другие ключи настроек:

```
postgres@postgresdoc:~$ man createuser
```

Роль создана, поднимаем БД.

### Создание базы данных

Любому созданному аккаунту привязывается база данных с идентичным именем, то есть наш созданный tester начнет подключаться к базе данных tester.  
Командой `createdb` добавим БД (поднимем новую базу PostgreSQL на Ubuntu), назвав ее `dev` (для удобства называем базу аналогично имени пользователя):

```
postgres@postgresdoc:~$ createdb dev
```

Вариант работы без переходов между аккаунтами:

```
$ sudo -u postgres createdb dev
```

### Переход в командную строку PostgreSQL с новой ролью

Работа в консоли PostgreSQL подразумевает наличие аккаунта Ubuntu с именем БД в Postgres. 

Добавим аккаунт Ubuntu, используя `adduser` (предварительно выйдя из аккаунта `postgres`), назвав аналогично новой роли:

```
$ sudo adduser tester
```

Добавив аккаунт tester, переключаемся на него и подключаемся к консоли:

```
$ sudo -i -u tester $ psql
```

второй вариант:

```
$ sudo -u tester psql
```

Переключиться на другую БД:

```
$ psql -d postgres
```

Проверка статуса:

```
tester=# \conninfo
```

