---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "11585987723"
reuse_id: "11596465547"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/11585987723"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Сетевые устройства / Huawei VRP USG: настройка актива"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Huawei VRP USG: настройка актива

> [!info] Раздел: System.Collections.Hashtable[@{Id=11585987723; ReuseId=11596465547; Title=Huawei VRP USG: настройка актива; Depth=3; Path=Справочник по настройке активов / Сетевые устройства / Huawei VRP USG: настройка актива; Segments=System.Object[]; Index=374}.Id])
> @{Id=11585987723; ReuseId=11596465547; Title=Huawei VRP USG: настройка актива; Depth=3; Path=Справочник по настройке активов / Сетевые устройства / Huawei VRP USG: настройка актива; Segments=System.Object[]; Index=374}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/11585987723)

---

Настройку актива нужно выполнять от имени учетной записи с правом перехода в режим глобальной конфигурации.

Для проведения аудита на активе нужно создать учетную запись для доступа MP 10 Collector по протоколу SSH версии 2.

**Задача.** Чтобы создать учетную запись для доступа к активу:

1. На узле, с которого производится настройка источника, запустите терминальный клиент, поддерживающий SSH.
2. Пройдите аутентификацию на активе.
3. Перейдите в режим конфигурирования:
   ```
   system-view
   ```
4. Создайте учетную запись для доступа к активу:
   ```
   aaa
   manager-user <Логин>
   password cipher <Пароль>
   service-type ssh
   level 15
   quit
   bind manager-user <Логин> role system-admin
   ```
   > [!note] Примечание
   > Для соблюдения требований ИБ можно ограничить выполнение команд, оставив разрешение только на display \*, где \* — любые параметры.
5. Выйдите из режима конфигурирования:
   ```
   quit
   ```
6. Сохраните изменения:
   ```
   save
   ```
