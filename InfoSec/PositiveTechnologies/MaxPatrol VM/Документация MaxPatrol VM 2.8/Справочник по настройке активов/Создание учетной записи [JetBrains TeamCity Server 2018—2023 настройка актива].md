---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "10271660043"
reuse_id: "10274503947"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/10271660043"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Другие активы / JetBrains TeamCity Server 2018—2023: настройка актива / Создание учетной записи"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание учетной записи

> [!info] Раздел: System.Collections.Hashtable[@{Id=10271660043; ReuseId=10274503947; Title=Создание учетной записи; Depth=4; Path=Справочник по настройке активов / Другие активы / JetBrains TeamCity Server 2018—2023: настройка актива / Создание учетной записи; Segments=System.Object[]; Index=511}.Id])
> @{Id=10271660043; ReuseId=10274503947; Title=Создание учетной записи; Depth=4; Path=Справочник по настройке активов / Другие активы / JetBrains TeamCity Server 2018—2023: настройка актива / Создание учетной записи; Segments=System.Object[]; Index=511}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/10271660043)

---

**Задача.** Чтобы создать учетную запись для доступа к активу:

1. Войдите в веб-интерфейс JetBrains TeamCity под учетной записью с правами администратора.
2. В главном меню выберите **Administration**.
3. Нажмите **Users**.
4. Нажмите **Create user account**.
5. Введите имя учетной записи.
6. Введите пароль и подтвердите его.
7. Нажмите **Create User**.
8. В профиле учетной записи в разделе **Roles** нажмите **Assign Role**.
9. В раскрывающемся списке **Roles** выберите [[Создание и настройка роли [JetBrains TeamCity Server 2018—2023 настройка актива]|созданную роль]].
10. В раскрывающемся списке **Scope** выберите **<Root project>**.
11. Установите флажок **Replace all existing roles with the newly selected role**.
12. Нажмите **Assign**.
