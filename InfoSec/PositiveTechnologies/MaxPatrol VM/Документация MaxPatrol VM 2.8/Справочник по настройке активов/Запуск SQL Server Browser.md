---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "218766731"
reuse_id: "3552300299"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/218766731"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Стандартные операции для настройки активов / Настройка доступа в СУБД Microsoft SQL Server / Запуск SQL Server Browser"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Запуск SQL Server Browser

> [!info] Раздел: System.Collections.Hashtable[@{Id=218766731; ReuseId=3552300299; Title=Запуск SQL Server Browser; Depth=4; Path=Справочник по настройке активов / Стандартные операции для настройки активов / Настройка доступа в СУБД Microsoft SQL Server / Запуск SQL Server Browser; Segments=System.Object[]; Index=568}.Id])
> @{Id=218766731; ReuseId=3552300299; Title=Запуск SQL Server Browser; Depth=4; Path=Справочник по настройке активов / Стандартные операции для настройки активов / Настройка доступа в СУБД Microsoft SQL Server / Запуск SQL Server Browser; Segments=System.Object[]; Index=568}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/218766731)

---

**Задача.** Чтобы настроить автоматический запуск службы SQL Server Browser:

1. Запустите SQL Server Configuration Manager.
2. В левой части открывшегося окна выберите узел **SQL Server Configuration Manager (Local)** → **SQL Server Services**.
3. В контекстном меню **SQL Server Browser** выберите пункт **Свойства**.
4. В открывшемся окне **Свойства: SQL Server Browser** выберите вкладку **Service**.
5. В раскрывающемся списке **Start Mode** выберите **Automatic**.
6. Выберите вкладку **Log On**.
7. Нажмите кнопку **Start** для запуска службы SQL Server Browser.
8. Нажмите кнопку **ОК**.
9. Перезапустите СУБД.

> Автоматический запуск службы SQL Server Browser настроен.
