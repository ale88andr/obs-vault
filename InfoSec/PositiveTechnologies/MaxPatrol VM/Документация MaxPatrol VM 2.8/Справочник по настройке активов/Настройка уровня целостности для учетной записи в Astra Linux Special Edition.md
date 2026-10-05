---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "7889803531"
reuse_id: "7890914187"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7889803531"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Стандартные операции для настройки активов / Стандартные операции в ОС семейства Unix / Настройка уровня целостности для учетной записи в Astra Linux Special Edition"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Настройка уровня целостности для учетной записи в Astra Linux Special Edition

> [!info] Раздел: System.Collections.Hashtable[@{Id=7889803531; ReuseId=7890914187; Title=Настройка уровня целостности для учетной записи в Astra Linux Special Edition; Depth=4; Path=Справочник по настройке активов / Стандартные операции для настройки активов / Стандартные операции в ОС семейства Unix / Настройка уровня целостности для учетной записи в Astra Linux Special Edition; Segments=System.Object[]; Index=536}.Id])
> @{Id=7889803531; ReuseId=7890914187; Title=Настройка уровня целостности для учетной записи в Astra Linux Special Edition; Depth=4; Path=Справочник по настройке активов / Стандартные операции для настройки активов / Стандартные операции в ОС семейства Unix / Настройка уровня целостности для учетной записи в Astra Linux Special Edition; Segments=System.Object[]; Index=536}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7889803531)

---

> [!warning] Внимание
> Настройку нужно выполнять от имени учетной записи root.

**Задача.** Чтобы настроить уровень целостности в Astra Linux Special Edition:

1. Получите значение максимального уровня целостности:
   ```bash
   cat /sys/module/parsec/parameters/max_ilev
   ```
2. Задайте полученное значение для требуемого пользователя:
   ```
   pdpl-user -i <Значение> <Логин>
   ```
