---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "5923916171"
reuse_id: "5924051211"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5923916171"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы управления базами данных / MariaDB 10.0 и выше: настройка актива / Настройка удаленного доступа в MariaDB"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Настройка удаленного доступа в MariaDB

> [!info] Раздел: System.Collections.Hashtable[@{Id=5923916171; ReuseId=5924051211; Title=Настройка удаленного доступа в MariaDB; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / MariaDB 10.0 и выше: настройка актива / Настройка удаленного доступа в MariaDB; Segments=System.Object[]; Index=445}.Id])
> @{Id=5923916171; ReuseId=5924051211; Title=Настройка удаленного доступа в MariaDB; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / MariaDB 10.0 и выше: настройка актива / Настройка удаленного доступа в MariaDB; Segments=System.Object[]; Index=445}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5923916171)

---

**Задача.** Чтобы настроить удаленный доступ в СУБД:

1. Откройте конфигурационный файл `my.cnf`.
   > [!note] Примечание
   > Вы можете узнать расположение файла, выполнив команду `mysqld --help --verbose | grep my.cnf`.
2. В секцию `[mysqld]` добавьте строки:
   ```
   port = 3306
   bind_address = <IP-адрес СУБД источника>
   ```
3. Сохраните конфигурационный файл.
4. Перезапустите СУБД:
   ```bash
   systemctl restart mysqld
   ```

> Удаленный доступ в СУБД настроен.
