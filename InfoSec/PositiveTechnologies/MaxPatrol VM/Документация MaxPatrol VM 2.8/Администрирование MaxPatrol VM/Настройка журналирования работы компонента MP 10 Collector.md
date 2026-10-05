---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "2093197067"
reuse_id: "2093465483"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/2093197067"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Настройка журналирования работы MaxPatrol VM / Настройка журналирования работы компонента MP 10 Collector"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Настройка журналирования работы компонента MP 10 Collector

> [!info] Раздел: System.Collections.Hashtable[@{Id=2093197067; ReuseId=2093465483; Title=Настройка журналирования работы компонента MP 10 Collector; Depth=3; Path=Администрирование MaxPatrol VM / Настройка журналирования работы MaxPatrol VM / Настройка журналирования работы компонента MP 10 Collector; Segments=System.Object[]; Index=82}.Id])
> @{Id=2093197067; ReuseId=2093465483; Title=Настройка журналирования работы компонента MP 10 Collector; Depth=3; Path=Администрирование MaxPatrol VM / Настройка журналирования работы MaxPatrol VM / Настройка журналирования работы компонента MP 10 Collector; Segments=System.Object[]; Index=82}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/2093197067)

---

Вы можете настраивать уровень журналирования и параметры ротации файлов журнала. По умолчанию в журнал записываются события уровня `DEBUG`. Размер каждого файла журнала ограничен 100 МБ, сохраняются последние 50 файлов.

Для настройки журналирования вам потребуется внести изменения в файл, расположенный на сервере MP 10 Collector:

- Если MP 10 Collector установлен на Microsoft Windows — `C:\Program Files (x86)\Positive Technologies\MP 10 Collector\agent.log.xml`;
- Если MP 10 Collector установлен на Linux — `/opt/core-agent/agent.log.xml`.

> [!note] Примечание
> Не рекомендуется изменять уровень журналирования без указания службы технической поддержки Positive Technologies.

**Задача.** Чтобы настроить журналирование:

1. В файле `agent.log.xml` измените значение атрибута `level` параметра `config` → `root`:
   ```xml
   <Название журналируемого компонента коллектора> level="<Уровень журналирования>"
   ```
   > [!note] Примечание
   > Возможные значения NOTSET, FATAL, ERROR, WARN, INFO, DEBUG и TRACE.
2. Измените значения атрибутов `max_file_size` и `max_backup_index` параметра`config` → `params`:
   ```
   params max_file_size="<Максимальный размер файла журнала (в мегабайтах)>" max_backup_index="<Максимальное количество сохраняемых файлов журналов>"
   ```
3. Перезапустите службу Core Agent.

> Журналирование настроено.

Эта инструкция не предназначена для настройки журналирования работы модулей MP 10 Collector и их компонентов. Оно настраивается с помощью справочников MaxPatrol VM.
