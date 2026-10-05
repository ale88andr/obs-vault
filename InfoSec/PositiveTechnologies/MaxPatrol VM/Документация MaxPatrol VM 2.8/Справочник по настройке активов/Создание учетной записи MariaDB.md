---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "5921888523"
reuse_id: "5922120075"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5921888523"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы управления базами данных / MariaDB 10.0 и выше: настройка актива / Создание учетной записи MariaDB"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание учетной записи MariaDB

> [!info] Раздел: System.Collections.Hashtable[@{Id=5921888523; ReuseId=5922120075; Title=Создание учетной записи MariaDB; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / MariaDB 10.0 и выше: настройка актива / Создание учетной записи MariaDB; Segments=System.Object[]; Index=446}.Id])
> @{Id=5921888523; ReuseId=5922120075; Title=Создание учетной записи MariaDB; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / MariaDB 10.0 и выше: настройка актива / Создание учетной записи MariaDB; Segments=System.Object[]; Index=446}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5921888523)

---

**Задача.** Чтобы создать учетную запись пользователя СУБД:

1. Откройте интерфейс командной строки актива.
2. Создайте учетную запись для доступа к активу:
   ```
   CREATE USER '<Логин>'@'<IP-адрес или FQDN сервера>' IDENTIFIED BY '<Пароль>';
   ```
3. Предоставьте учетной записи права на чтение БД:
   ```
   GRANT SELECT ON mysql.* TO '<Логин>'@'<IP-адрес или FQDN сервера>';
   GRANT SHOW DATABASES ON *.* TO '<Логин>'@'<IP-адрес или FQDN сервера>';
   GRANT SHOW VIEW ON *.* TO '<Логин>'@'<IP-адрес или FQDN сервера>';
   FLUSH PRIVIlEGES;
   ```

> Учетная запись СУБД создана.
