---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "8030515851"
reuse_id: "8491203595"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/8030515851"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Стандартные операции для настройки активов / Стандартные операции в системах виртуализации VMware / Добавление учетной записи и назначение роли в VMware vCenter Server for Windows 5.5, 6.0"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Добавление учетной записи и назначение роли в VMware vCenter Server for Windows 5.5, 6.0

> [!info] Раздел: System.Collections.Hashtable[@{Id=8030515851; ReuseId=8491203595; Title=Добавление учетной записи и назначение роли в VMware vCenter Server for Windows 5.5, 6.0; Depth=4; Path=Справочник по настройке активов / Стандартные операции для настройки активов / Стандартные операции в системах виртуализации VMware / Добавление учетной записи и назначение роли в VMware vCenter Server for Windows 5.5, 6.0; Segments=System.Object[]; Index=569}.Id])
> @{Id=8030515851; ReuseId=8491203595; Title=Добавление учетной записи и назначение роли в VMware vCenter Server for Windows 5.5, 6.0; Depth=4; Path=Справочник по настройке активов / Стандартные операции для настройки активов / Стандартные операции в системах виртуализации VMware / Добавление учетной записи и назначение роли в VMware vCenter Server for Windows 5.5, 6.0; Segments=System.Object[]; Index=569}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/8030515851)

---

**Задача.** Чтобы добавить учетную запись в VMware vCenter Server for Windows и назначить ей роль:

1. Войдите в веб-интерфейс VMware vCenter Server for Windows от имени учетной записи с правами администратора.
2. В иерархическом списке выберите узел сервера.
3. Выберите вкладку **Manage**.
4. Нажмите **Permissions**.
5. В панели инструментов нажмите ![[144982155.png]].
6. В открывшемся окне нажмите **Add**.
7. Если используется доменная учетная запись, в раскрывающемся списке **Domain** выберите домен учетной записи.
8. В списке выберите учетную запись и нажмите **Add**.
9. Нажмите **ОК**.
10. В окне **<Имя сервера> — Change Role On Permissions** в раскрывающемся списке выберите **Read-only**.
11. Нажмите **ОК**.
