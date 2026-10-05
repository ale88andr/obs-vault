---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "7919060235"
reuse_id: "7919405835"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7919060235"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Привилегии и роли MaxPatrol VM"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Привилегии и роли MaxPatrol VM

> [!info] Раздел: System.Collections.Hashtable[@{Id=7919060235; ReuseId=7919405835; Title=Привилегии и роли MaxPatrol VM; Depth=2; Path=Администрирование MaxPatrol VM / Привилегии и роли MaxPatrol VM; Segments=System.Object[]; Index=130}.Id])
> @{Id=7919060235; ReuseId=7919405835; Title=Привилегии и роли MaxPatrol VM; Depth=2; Path=Администрирование MaxPatrol VM / Привилегии и роли MaxPatrol VM; Segments=System.Object[]; Index=130}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7919060235)

---

При изменении роли коды добавленных и удаленных привилегий отображаются на странице **Журнал действий пользователя** в приложении Management and Configuration.

**Привилегии и роли MaxPatrol VM**

| Код привилегии | Привилегия | Администратор | Оператор | Наблюдатель |
| --- | --- | --- | --- | --- |
| **Assets** | **Активы** |   |   |   |
| Vulners | Уязвимости | + | + | - |
| Assets | Создание, просмотр, изменение, удаление | + | + | - |
| AssetsRead | Просмотр | - | - | + |
| **Common** | **Общее** |   |   |   |
| AccessAdmin | Расширенные полномочия | + | - | - |
| **DataCollect** | **Сбор данных** |   |   |   |
| **DataCollectTasks** | **Задачи** |   |   |   |
| DataCollectTasks | Создание, просмотр, изменение, удаление | + | + | - |
| DataCollectTasksRead | Просмотр | - | - | + |
| **DataCollectProfiles** | **Профили** |   |   |   |
| DataCollectProfiles | Создание, просмотр, изменение, удаление | + | + | - |
| DataCollectProfilesRead | Просмотр | - | - | + |
| **DataCollectIdentity** | **Учетные записи** |   |   |   |
| DataCollectIdentity | Создание, просмотр, изменение, удаление | + | + | - |
| DataCollectIdentityRead | Просмотр | - | - | + |
| **DataCollectReferences** | **Справочники** |   |   |   |
| DataCollectReferences | Создание, просмотр, изменение, удаление | + | + | - |
| DataCollectReferencesRead | Просмотр | - | - | + |
| **DataCollectExclusions** | **Исключения** |   |   |   |
| DataCollectExclusionsViewing | Просмотр | - | - | + |
| DataCollectExclusionsEditing | Создание, просмотр, изменение, удаление | + | + | - |
| **Infrastructure** | **Инфраструктура** | + | - | - |
| **System** | **Система** |   |   |   |
| AccessRights | Права доступа | + | - | - |
| Triggers | Уведомления | + | + | - |
| SystemManagement | Управление системой | + | + | - |
| ChecksManagement | Управление контролями | + | + | - |
| Reports | Отчеты | + | + | - |
| **Topology** | **Топология** |   |   |   |
| TopologyAnalyzer | Расчет достижимости | + | + | - |
| Topology | Топология | + | + | - |
