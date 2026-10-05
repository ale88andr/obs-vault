---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "1454182411"
reuse_id: "5924375435"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/1454182411"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы управления базами данных / Oracle MySQL 5.7 и выше: настройка актива / Настройка удаленного доступа в MySQL"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Настройка удаленного доступа в MySQL

> [!info] Раздел: System.Collections.Hashtable[@{Id=1454182411; ReuseId=5924375435; Title=Настройка удаленного доступа в MySQL; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / Oracle MySQL 5.7 и выше: настройка актива / Настройка удаленного доступа в MySQL; Segments=System.Object[]; Index=459}.Id])
> @{Id=1454182411; ReuseId=5924375435; Title=Настройка удаленного доступа в MySQL; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / Oracle MySQL 5.7 и выше: настройка актива / Настройка удаленного доступа в MySQL; Segments=System.Object[]; Index=459}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/1454182411)

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
