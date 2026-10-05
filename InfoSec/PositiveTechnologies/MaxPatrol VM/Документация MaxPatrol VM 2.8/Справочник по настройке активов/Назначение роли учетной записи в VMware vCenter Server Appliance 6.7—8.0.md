---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "8030755467"
reuse_id: "8491205899"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/8030755467"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Стандартные операции для настройки активов / Стандартные операции в системах виртуализации VMware / Назначение роли учетной записи в VMware vCenter Server Appliance 6.7—8.0"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Назначение роли учетной записи в VMware vCenter Server Appliance 6.7—8.0

> [!info] Раздел: System.Collections.Hashtable[@{Id=8030755467; ReuseId=8491205899; Title=Назначение роли учетной записи в VMware vCenter Server Appliance 6.7—8.0; Depth=4; Path=Справочник по настройке активов / Стандартные операции для настройки активов / Стандартные операции в системах виртуализации VMware / Назначение роли учетной записи в VMware vCenter Server Appliance 6.7—8.0; Segments=System.Object[]; Index=572}.Id])
> @{Id=8030755467; ReuseId=8491205899; Title=Назначение роли учетной записи в VMware vCenter Server Appliance 6.7—8.0; Depth=4; Path=Справочник по настройке активов / Стандартные операции для настройки активов / Стандартные операции в системах виртуализации VMware / Назначение роли учетной записи в VMware vCenter Server Appliance 6.7—8.0; Segments=System.Object[]; Index=572}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/8030755467)

---

**Задача.** Чтобы назначить роль учетной записи:

1. Войдите в веб-интерфейс VMware vCenter Server Appliance от имени учетной записи с правами администратора.
2. В левой части страницы нажмите **Hosts and Clusters**.
3. В иерархическом списке выберите узел сервера.
4. Выберите вкладку **Permissions**.
5. В панели инструментов вкладки нажмите .
6. В раскрывающемся списке **User** выберите домен и в поле ниже введите логин учетной записи.
7. В раскрывающемся списке **Role** выберите **Read-only**.
8. Установите флажок **Propagate to children**.
9. Нажмите **ОК**.
