---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "3249942923"
reuse_id: "3664782091"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/3249942923"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Настройка журналирования работы MaxPatrol VM / Настройка журналирования работы компонента MP 10 Core на Linux"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Настройка журналирования работы компонента MP 10 Core на Linux

> [!info] Раздел: System.Collections.Hashtable[@{Id=3249942923; ReuseId=3664782091; Title=Настройка журналирования работы компонента MP 10 Core на Linux; Depth=3; Path=Администрирование MaxPatrol VM / Настройка журналирования работы MaxPatrol VM / Настройка журналирования работы компонента MP 10 Core на Linux; Segments=System.Object[]; Index=81}.Id])
> @{Id=3249942923; ReuseId=3664782091; Title=Настройка журналирования работы компонента MP 10 Core на Linux; Depth=3; Path=Администрирование MaxPatrol VM / Настройка журналирования работы MaxPatrol VM / Настройка журналирования работы компонента MP 10 Core на Linux; Segments=System.Object[]; Index=81}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/3249942923)

---

Настройка выполняется отдельно для каждой службы компонента.

**Задача.** Чтобы настроить журналирование:

1. На сервере MP 10 Core в файл `/var/lib/deployed-roles/<Идентификатор приложения MaxPatrol VM>/<Название экземпляра роли Core>/images/<Название службы>/config/custom.env` добавьте параметр:
   ```
   Logging_Threshold=<Уровень журналирования>
   ```
   > [!note] Примечание
   > Возможны значения FATAL, ERROR, WARN, INFO, DEBUG и TRACE.
2. Выполните команды:
   ```bash
   cd /var/lib/deployed-roles/<Идентификатор приложения MaxPatrol VM>/<Название экземпляра роли Core>/images/<Название службы>/
   docker-compose down
   docker-compose up -d
   ```

> Журналирование настроено.
