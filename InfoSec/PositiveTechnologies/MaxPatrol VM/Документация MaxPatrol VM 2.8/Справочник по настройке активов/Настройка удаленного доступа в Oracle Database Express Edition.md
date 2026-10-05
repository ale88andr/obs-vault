---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "431959051"
reuse_id: "5924372747"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/431959051"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Системы управления базами данных / Oracle Database 11, 12, 18, 19, 21, 23: настройка актива / Настройка удаленного доступа в Oracle Database Express Edition"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Настройка удаленного доступа в Oracle Database Express Edition

> [!info] Раздел: System.Collections.Hashtable[@{Id=431959051; ReuseId=5924372747; Title=Настройка удаленного доступа в Oracle Database Express Edition; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / Oracle Database 11, 12, 18, 19, 21, 23: настройка актива / Настройка удаленного доступа в Oracle Database Express Edition; Segments=System.Object[]; Index=455}.Id])
> @{Id=431959051; ReuseId=5924372747; Title=Настройка удаленного доступа в Oracle Database Express Edition; Depth=4; Path=Справочник по настройке активов / Системы управления базами данных / Oracle Database 11, 12, 18, 19, 21, 23: настройка актива / Настройка удаленного доступа в Oracle Database Express Edition; Segments=System.Object[]; Index=455}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/431959051)

---

**Задача.** Чтобы настроить удаленный доступ в СУБД:

1. Остановите службу `Oracle<Имя сервера СУБД>TNSListener`.
2. В файле `sqlnet.ora` укажите для параметра `SQLNET.AUTHENTICATION_SERVICES` значение `(NONE)`.
   > [!note] Примечание
   > Параметр `SQLNET.AUTHENTICATION_SERVICES=(NONE)` отключает аутентификацию на основе операционной системы.
3. В файле `listener.ora` укажите для параметра `HOST` значение`$GLOBAL_HOST_NAME`:
   ```
   (ADDRESS = (PROTOCOL = TCP)(HOST = $GLOBAL_HOST_NAME)(PORT = 1521))
   ```
4. Запустите службу СУБД.

> Удаленный доступ в СУБД настроен.
