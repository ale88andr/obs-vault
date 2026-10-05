---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "473586699"
reuse_id: "3557426827"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/473586699"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Стандартные операции для настройки активов / Стандартные операции в ОС семейства Unix / Перезапуск службы в ОС семейства Unix"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Перезапуск службы в ОС семейства Unix

> [!info] Раздел: System.Collections.Hashtable[@{Id=473586699; ReuseId=3557426827; Title=Перезапуск службы в ОС семейства Unix; Depth=4; Path=Справочник по настройке активов / Стандартные операции для настройки активов / Стандартные операции в ОС семейства Unix / Перезапуск службы в ОС семейства Unix; Segments=System.Object[]; Index=533}.Id])
> @{Id=473586699; ReuseId=3557426827; Title=Перезапуск службы в ОС семейства Unix; Depth=4; Path=Справочник по настройке активов / Стандартные операции для настройки активов / Стандартные операции в ОС семейства Unix / Перезапуск службы в ОС семейства Unix; Segments=System.Object[]; Index=533}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/473586699)

---

**Задача.** Чтобы перезапустить службу в ОС семейства Unix,

1. выполните команду:
   - если в ОС используется система инициализации SysV:
   ```
   /etc/init.d/<Имя службы> restart
   ```
   - если используется BSD-style init:
   ```
   /etc/rc.d/<Имя службы> restart
   ```
   - если Upstart:
   ```bash
   service <Имя службы> restart
   ```
   - если systemd:
   ```bash
   systemctl restart <Имя службы>
   ```

> Служба перезапущена.
