---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "6983363339"
reuse_id: "7713576715"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6983363339"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Диагностика и решение проблем / Задача на сбор данных с коллектором на Windows завершается с ошибкой"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Задача на сбор данных с коллектором на Windows завершается с ошибкой

> [!info] Раздел: System.Collections.Hashtable[@{Id=6983363339; ReuseId=7713576715; Title=Задача на сбор данных с коллектором на Windows завершается с ошибкой; Depth=3; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Задача на сбор данных с коллектором на Windows завершается с ошибкой; Segments=System.Object[]; Index=108}.Id])
> @{Id=6983363339; ReuseId=7713576715; Title=Задача на сбор данных с коллектором на Windows завершается с ошибкой; Depth=3; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Задача на сбор данных с коллектором на Windows завершается с ошибкой; Segments=System.Object[]; Index=108}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6983363339)

---

Журнал выбранной подзадачи содержит следующую информацию об ошибке:

```
ERROR Agent.JobService: Failed to start module! Descr: Can't start process!
 [Nested error] = Can't start child process
  [Nested error] = CreateProcess failed: The system cannot find the file specified.
```

## Возможные причины

Возможной причиной проблемы является то, что запускаемый файл процесса ModuleHost.exe помещен в карантин антивирусной программой Microsoft Defender.

## Решение

**Задача.** Чтобы решить проблему,

1. восстановите файл из карантина.

Подробную инструкцию см. на сайте [learn.microsoft.com](https://learn.microsoft.com/) в разделе «Восстановление файлов в карантине в антивирусной программе Microsoft Defender».
