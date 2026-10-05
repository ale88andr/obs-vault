---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "5921891723"
reuse_id: "5924573963"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5921891723"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы управления базами данных / Oracle MySQL 5.7 и выше: настройка актива / Создание учетной записи Oracle MySQL"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание учетной записи Oracle MySQL

> [!info] Раздел: System.Collections.Hashtable[@{Id=5921891723; ReuseId=5924573963; Title=Создание учетной записи Oracle MySQL; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / Oracle MySQL 5.7 и выше: настройка актива / Создание учетной записи Oracle MySQL; Segments=System.Object[]; Index=460}.Id])
> @{Id=5921891723; ReuseId=5924573963; Title=Создание учетной записи Oracle MySQL; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / Oracle MySQL 5.7 и выше: настройка актива / Создание учетной записи Oracle MySQL; Segments=System.Object[]; Index=460}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5921891723)

---

**Задача.** Чтобы создать учетную запись пользователя СУБД:

1. Откройте интерфейс командной строки актива.
2. Создайте учетную запись для доступа к активу:
   ```
   CREATE USER '<Логин>'@'<IP-адрес или FQDN сервера>' IDENTIFIED BY '<Пароль>';
   ```
3. Предоставьте учетной записи права на чтение БД:
   ```
   GRANT SELECT ON performance_schema.global_variables TO '<Логин>'@'<Имя актива>';
   GRANT SELECT ON mysql.* TO '<Логин>'@'<IP-адрес или FQDN сервера>';
   GRANT SHOW DATABASES ON *.* TO '<Логин>'@'<IP-адрес или FQDN сервера>';
   GRANT SHOW VIEW ON *.* TO '<Логин>'@'<IP-адрес или FQDN сервера>';
   FLUSH PRIVIlEGES;
   ```
4. Если используется Oracle MySQL версии 8.0.21 и выше, предоставьте учетной записи права на чтение таблицы tls_channel_status:
   ```
   GRANT SELECT ON performance_schema.tls_channel_status TO '<Логин>'@'<Имя актива>';
   ```

> Учетная запись СУБД создана.
