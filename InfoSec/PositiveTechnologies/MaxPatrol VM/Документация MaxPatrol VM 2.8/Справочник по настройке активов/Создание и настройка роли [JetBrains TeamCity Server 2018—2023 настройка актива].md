---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "10271658891"
reuse_id: "10274505483"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/10271658891"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Другие активы / JetBrains TeamCity Server 2018—2023: настройка актива / Создание и настройка роли"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание и настройка роли

> [!info] Раздел: System.Collections.Hashtable[@{Id=10271658891; ReuseId=10274505483; Title=Создание и настройка роли; Depth=4; Path=Справочник по настройке активов / Другие активы / JetBrains TeamCity Server 2018—2023: настройка актива / Создание и настройка роли; Segments=System.Object[]; Index=510}.Id])
> @{Id=10271658891; ReuseId=10274505483; Title=Создание и настройка роли; Depth=4; Path=Справочник по настройке активов / Другие активы / JetBrains TeamCity Server 2018—2023: настройка актива / Создание и настройка роли; Segments=System.Object[]; Index=510}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/10271658891)

---

**Задача.** Чтобы создать и настроить роль:

1. Войдите в веб-интерфейс JetBrains TeamCity под учетной записью с правами администратора.
2. В главном меню выберите **Administration**.
3. Нажмите **Roles**.
4. Нажмите **Create new role**.
5. Введите название роли.
6. Нажмите **Create**.
7. В списке ролей выберите созданную роль и нажмите **Add permission**.
8. Назначьте роли следующие привилегии:
   - **View project and all parent projects**;
   - **View build configuration settings**;
   - **View build runtime parameters and data**;
   - **Change server settings**;
   - **View agent details**;
   - **View agent usage statistics**;
   - **View user profile**;
   - **View all registered users**;
   - **View project agents details**.
