---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "5189890187"
reuse_id: "5225874827"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5189890187"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Другие активы / JetBrains Hub 2018.1—2022: настройка актива / Создание и настройка роли"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание и настройка роли

> [!info] Раздел: System.Collections.Hashtable[@{Id=5189890187; ReuseId=5225874827; Title=Создание и настройка роли; Depth=4; Path=Справочник по настройке активов / Другие активы / JetBrains Hub 2018.1—2022: настройка актива / Создание и настройка роли; Segments=System.Object[]; Index=505}.Id])
> @{Id=5189890187; ReuseId=5225874827; Title=Создание и настройка роли; Depth=4; Path=Справочник по настройке активов / Другие активы / JetBrains Hub 2018.1—2022: настройка актива / Создание и настройка роли; Segments=System.Object[]; Index=505}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5189890187)

---

**Задача.** Чтобы создать и настроить роль:

1. Войдите в веб-интерфейс JetBrains Hub под учетной записью с правами администратора.
2. Перейдите на страницу **Administration** → **Access Management** → **Roles**.
3. Нажмите кнопку **New role**.
4. В поле **Name** укажите название роли и нажмите кнопку **Create**.
   → Откроется окно с параметрами роли.
5. Выберите вкладку **Permissions**.
6. В столбце **Hub** установите следующие флажки:
   - **Low-level Admin Read**;
   - **Read Group**;
   - **Read Organization**;
   - **Read User Full**;
   - **Read Project Basic**;
   - **Read Project Full**;
   - **Read Role**.
   > [!note] Примечание
   > Параметры роли сохраняются автоматически.

> Роль создана и настроена.
