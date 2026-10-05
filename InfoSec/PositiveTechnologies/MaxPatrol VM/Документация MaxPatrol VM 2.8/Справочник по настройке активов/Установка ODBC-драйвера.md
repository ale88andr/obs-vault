---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "5967568523"
reuse_id: "5980431627"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5967568523"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Стандартные операции для настройки активов / Стандартные операции в ОС семейства Unix / Установка ODBC-драйвера"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Установка ODBC-драйвера

> [!info] Раздел: System.Collections.Hashtable[@{Id=5967568523; ReuseId=5980431627; Title=Установка ODBC-драйвера; Depth=4; Path=Справочник по настройке активов / Стандартные операции для настройки активов / Стандартные операции в ОС семейства Unix / Установка ODBC-драйвера; Segments=System.Object[]; Index=534}.Id])
> @{Id=5967568523; ReuseId=5980431627; Title=Установка ODBC-драйвера; Depth=4; Path=Справочник по настройке активов / Стандартные операции для настройки активов / Стандартные операции в ОС семейства Unix / Установка ODBC-драйвера; Segments=System.Object[]; Index=534}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5967568523)

---

Для подключения к СУБД на узле MP 10 Collector нужно установить ODBC-драйвер. Для узлов с ОС семейства Linux необходимо использовать 64-разрядную версию. Если в инструкции не указана версия драйвера, рекомендуется использовать наиболее актуальную версию из доступных.

> [!warning] Внимание
> Инструкции разработаны на основе документации вендоров СУБД и предназначены для установки ODBC-драйвера на узел с Debian 12. В других ОС команды и пути к файлам могут отличаться.

## Подготовка к установке

> [!warning] Внимание
> По этой инструкции можно подготовить к установке ODBC-драйвера только узел с Debian 12.

**Задача.** Чтобы подготовить узел к установке ODBC-драйвера:

1. Откройте файл `/etc/apt/sources.list`.
2. Замените все строки в файле на следующие:
   ```
   deb http://deb.debian.org/debian bookworm main
   deb-src http://deb.debian.org/debian bookworm main
   deb http://security.debian.org/debian-security bookworm-security main
   deb-src http://security.debian.org/debian-security bookworm-security main
   deb http://deb.debian.org/debian bookworm-updates main
   deb-src http://deb.debian.org/debian bookworm-updates main
   ```
3. Сохраните изменения и закройте файл.
4. Обновите данные о пакетах:
   ```bash
   apt update
   ```
5. Установите пакеты unixodbc-dev, freetds-dev, unixodbc и odbcinst, необходимые для сборки и запуска транспорта в Debian:
   ```bash
   apt install freetds-dev unixodbc-dev unixodbc odbcinst
   ```

## Для СУБД Firebird

**Задача.** Чтобы установить ODBC-драйвер:

1. Скачайте с сайта [firebirdsql.org](https://firebirdsql.org/en/odbc-driver/), распакуйте и установите ODBC-драйвер.
   → Например:
   ```bash
   wget https://sourceforge.net/projects/firebird/files/firebird-ODBC-driver/2.0.5-Release/OdbcFb-LIB-2.0.5.156.amd64.gz/download -O - | tar –xz
   ```
2. Переместите файл `libOdbcFb.so` в каталог для библиотек:
   ```bash
   mv libOdbcFb.so /usr/lib64/libOdbcFb.so
   ```
3. Запустите установку библиотеки libfbclient2:
   ```bash
   apt install libfbclient2
   ```
4. Создайте символическую ссылку для библиотеки libfbclient:
   ```
   ln -s /usr/lib/x86_64-linux-gnu/firebird/3.0/lib/libfbclient.so.2 /usr/lib/x86_64-linux-gnu/libgds.so
   ```
5. Откройте файл `/etc/odbcinst.ini`.
6. Добавьте в файл строки:
   ```
   [Firebird]
   Description=InterBase/Firebird ODBC Driver
   Driver=/usr/lib64/libOdbcFb.so
   Setup=/usr/lib64/libOdbcFb.so
   Threading=1
   FileUsage=1
   CPTimeout=
   CPReuse=
   ```
7. Сохраните изменения и закройте файл.
8. Если требуется, проверьте подключение к СУБД:
   ```
   isql -k -v "Driver={Firebird};Uid=<Логин>;Pwd=<Пароль>;Dbname=<IP-адрес>/<Порт>:<Путь к файлу БД>;"
   ```
   → Например:
   ```
   isql -k -v "Driver={Firebird};Uid=SIEM_user;Pwd=P@ssw0rd;Dbname=10.10.10.10/3050:C:\Bastion\Data\BPROT.GDB;"
   ```
   → При успешном подключении появится сообщение `Connected`.

## Для СУБД Microsoft SQL Server

**Задача.** Чтобы установить ODBC-драйвер:

1. Добавьте репозиторий для Debian:
   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
   curl https://packages.microsoft.com/config/debian/<Версия Debian>/prod.list > /etc/apt/sources.list.d/mssql-release.list
   ```
2. Установите ODBC-драйвер:
   ```bash
   apt-get update
   ACCEPT_EULA=Y apt-get install -y msodbcsql17
   ```
3. Установите инструменты командной строки bcp и sqlcmd:
   ```
   ACCEPT_EULA=Y apt-get install -y mssql-tools
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```
4. Установите пакет с библиотекой libgssapi-krb5-2:
   ```bash
   apt-get install -y libgssapi-krb5-2
   ```
5. Если требуется, проверьте подключение к СУБД:
   ```
   isql -vv -k "Driver={ODBC Driver 17 for SQL Server};Server=<IP-адрес>;Database=<Название БД>;Uid=<Логин>;Pwd=<Пароль>;"
   ```
   → При успешном подключении появится сообщение `Connected`.

## Для СУБД MySQL

**Задача.** Чтобы установить ODBC-драйвер:

1. Скачайте с сайта [mysql.com](https://dev.mysql.com/downloads/connector/odbc/), распакуйте и установите ODBC-драйвер, например:
   ```bash
   wget https://dev.mysql.com/get/Downloads/Connector-ODBC/5.3/mysql-connector-odbc-5.3.14-linux-glibc2.12-x86-64bit.tar.gz -O - | tar -xz
   ```
2. Откройте файл `/etc/odbcinst.ini`.
3. Добавьте в файл строки, указав в параметре `Driver` пути к файлам ODBC-драйвера.
   → Например:
   ```
   [MySQL ODBC 5.3 Unicode Driver]
   Driver=/root/mysql-connector-odbc-5.3.14-linux-glibc2.12-x86-64bit/lib/libmyodbc5w.so
   UsageCount=1
   [MySQL ODBC 5.3 ANSI Driver]
   Driver=/root/mysql-connector-odbc-5.3.14-linux-glibc2.12-x86-64bit/lib/libmyodbc5a.so
   UsageCount=1
   ```
4. Сохраните изменения и закройте файл.
5. Если требуется, проверьте подключение к СУБД:
   ```
   isql -v -k "Driver={MySQL ODBC 5.3 ANSI Driver};Server=<IP-адрес>;Port=<Порт>;User=<Логин>;Password=<Пароль>;Charset=utf8;Option=3;"
   ```
   → Например:
   ```
   isql -v -k "Driver={MySQL ODBC 5.3 ANSI Driver};Server=10.0.10.10;Port=3306;User=SIEM_user;Password=P@ssw0rd;Charset=utf8;Option=3;"
   ```
   → При успешном подключении появится сообщение `Connected`.

## Для СУБД Oracle Database

**Задача.** Чтобы установить ODBC-драйвер:

1. Скачайте с сайта [oracle.com](https://www.oracle.com/cis/database/technologies/instant-client/linux-x86-64-downloads.html), распакуйте и установите набор инструментов разработчика Oracle Instant Client:
   ```bash
   mkdir oracle
   wget https://download.oracle.com/otn_software/linux/instantclient/217000/instantclient-basiclite-linux.x64-21.7.0.0.0dbru.zip
   unzip instantclient-basiclite-linux.x64-21.7.0.0.0dbru.zip -d oracle
   ```
2. Скачайте с сайта [oracle.com](https://www.oracle.com/cis/database/technologies/instant-client/linux-x86-64-downloads.html), распакуйте и установите ODBC-драйвер:
   ```bash
   wget https://download.oracle.com/otn_software/linux/instantclient/217000/instantclient-odbc-linux.x64-21.7.0.0.0dbru.zip
   unzip instantclient-odbc-linux.x64-21.7.0.0.0dbru.zip -d oracle
   ```
3. Настройте ODBC-драйвер:
   ```bash
   cd oracle/instantclient_21_7/
   ./odbc_update_ini.sh /
   ```
   > [!note] Примечание
   > Вы можете не обращать внимания на предупреждение `ODBCINI environment variable not set, defaulting it to HOME directory`.
   ```
   echo "/opt/oracle/instantclient_21_7" >> /etc/ld.so.conf.d/oracle_odbc.conf
   ldconfig
   cd ~
   ```
4. Если требуется, проверьте подключение к СУБД:
   ```
   isql -v -k "Driver={Oracle 21 ODBC driver};Dbq=<IP-адрес>:<Порт>/<Идентификатор системы>;Uid=<Логин>;Pwd=<Пароль>"
   ```
   → Например:
   ```
   isql -v -k "Driver={Oracle 21 ODBC driver};Dbq=10.10.10.10:1521/ORCL;Uid=SIEM_user;Pwd=Passw0rd"
   ```
   → При успешном подключении появится сообщение `Connected`.

## Для СУБД PostgreSQL

**Задача.** Чтобы установить ODBC-драйвер:

1. Установите пакет odbc-postgresql из дистрибутива ОС:
   ```bash
   apt install odbc-postgresql
   ```
2. Если требуется, проверьте подключение к СУБД:
   ```
   isql -v -k "Driver={PostgreSQL Unicode};Server=<IP-адрес>;Port=<Порт>;Uid=<Логин>;Pwd=<Пароль>;Database=<Название БД>;sslmode=prefer;pqopt={keepalives=1 keepalives_idle=5 keepalives_count=1 keepalives_interval=1};"
   ```
   → Например:
   ```
   isql -v -k "Driver={PostgreSQL Unicode};Server=10.10.10.10;Port=5432;Uid=SIEM_user;Pwd=P@ssw0rd;Database=drwcs;sslmode=prefer;pqopt={keepalives=1 keepalives_idle=5 keepalives_count=1 keepalives_interval=1};"
   ```
   → При успешном подключении появится сообщение `Connected`.

## Для СУБД ClickHouse

**Задача.** Чтобы установить ODBC-драйвер:

1. Скачайте с сайта [github.com](https://github.com/ClickHouse/clickhouse-odbc/releases/) архив `clickhouse-odbc-1.1.10-linux.tar.gz` с ODBC-драйвером версии 1.1.10.20210822.
2. Распакуйте полученный архив:
   ```
   tar -xf clickhouse-odbc-1.1.10-Linux.tar.gz
   ```
3. Перенесите файлы `libclickhouseodbc.so` и `libclickhouseodbcw.so` из полученного каталога `/clickhouse-odbc-1.1.10-Linux/lib64/` в каталог для установки, например `/var/lib/clickhouse-odbc/`:
   ```bash
   sudo mv clickhouse-odbc-1.1.10-Linux/lib64/* /var/lib/clickhouse-odbc/
   ```
4. Перенесите файлы `clickhouse-odbc.tdc.sample`, `odbc.ini.sample` и `odbcinst.ini.sample` из полученного каталога `/clickhouse-odbc-1.1.10-Linux/share/doc/clickhouse-odbc/config/` в каталог для установки, например `/var/lib/clickhouse-odbc/doc/`:
   ```bash
   sudo mv clickhouse-odbc-1.1.10-Linux/share/doc/clickhouse-odbc/config/* /var/lib/clickhouse-odbc/doc/
   ```
5. Откройте на редактирование файл `/var/lib/clickhouse-odbc/doc/odbcinst.ini.sample`.
6. Добавьте в файл строки:
   ```
   [ClickHouse ODBC Driver (ANSI)]
   Driver = /var/lib/clickhouse-odbc/libclickhouseodbc.so
   Setup = /var/lib/clickhouse-odbc/libclickhouseodbc.so
   [ClickHouse ODBC Driver (Unicode)]
   Driver = /var/lib/clickhouse-odbc/libclickhouseodbcw.so
   Setup = /var/lib/clickhouse-odbc/libclickhouseodbcw.so
   ```
7. Сохраните изменения и закройте файл.
8. Выполните команду:
   ```bash
   sudo apt install openssl unixodbc
   ```
9. Зарегистрируйте драйвер:
   ```bash
   sudo odbcinst -i -d -f /var/lib/clickhouse-odbc/doc/odbcinst.ini.sample
   sudo odbcinst -i -s -l -f /var/lib/clickhouse-odbc/doc/odbc.ini.sample
   ```
10. В конфигурационном файле `core-agent.conf` укажите путь к каталогу с библиотеками:
    ```bash
    sudo echo /usr/local/lib > /etc/ld.so.conf.d/core-agent.conf && ldconfig
    ```
11. Если драйвер устанавливается на Astra Linux, установите пакет execstack и отключите запрет на использование исполняемого стека для драйвера:
    ```bash
    sudo apt install execstack
    sudo execstack -c /usr/local/lib64/libclickhouseodbc.so
    ```
