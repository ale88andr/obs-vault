---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "6300519691"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6300519691"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Службы каталогов / Microsoft Active Directory в Windows Server 2003—2022: настройка MaxPatrol VM / Создание профиля для сбора данных Users"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание профиля для сбора данных Users

> [!info] Раздел: System.Collections.Hashtable[@{Id=6300519691; ReuseId=; Title=Создание профиля для сбора данных Users; Depth=4; Path=Справочник по настройке активов / Службы каталогов / Microsoft Active Directory в Windows Server 2003—2022: настройка MaxPatrol VM / Создание профиля для сбора данных Users; Segments=System.Object[]; Index=491}.Id])
> @{Id=6300519691; ReuseId=; Title=Создание профиля для сбора данных Users; Depth=4; Path=Справочник по настройке активов / Службы каталогов / Microsoft Active Directory в Windows Server 2003—2022: настройка MaxPatrol VM / Создание профиля для сбора данных Users; Segments=System.Object[]; Index=491}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6300519691)

---

**Задача.** Чтобы создать в MaxPatrol VM профиль для сбора данных Users:

1. В главном меню выберите **Сбор данных** → **Профили**.
   → Откроется страница **Профили**.
2. Выберите профиль **Microsoft Active Directory Audi**t.
3. Нажмите **Создать** → **На базе выбранного профиля**.
   → Откроется страница **Новый профиль**.
4. В поле **Название** введите `Users`.
5. В панели **Параметры профиля** установите флажок **Показывать дополнительные параметры**.
6. На вкладке **Область сбора данных** в раскрывающемся списке **Частичный сбор данных** выберите **По классам модели активов**.
7. В поле **Имена заполняемых классов** введите имена классов, которые будут заполняться для создаваемого профиля:
   ```
   DirectoryService.ForestTrust;DirectoryService.DomainTrust;DirectoryService.ActiveDirectory;DirectoryService.Domain;DirectoryService.ServicePrincipalName;DirectoryService.User
   ```
8. Нажмите кнопку **Сохранить**.

> Профиль для сбора данных Users создан.
