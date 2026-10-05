---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "7366061707"
reuse_id: "7394892555"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7366061707"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Сетевые устройства / MikroTik RouterOS 6, 7: настройка актива"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# MikroTik RouterOS 6, 7: настройка актива

> [!info] Раздел: System.Collections.Hashtable[@{Id=7366061707; ReuseId=7394892555; Title=MikroTik RouterOS 6, 7: настройка актива; Depth=3; Path=Справочник по настройке активов / Сетевые устройства / MikroTik RouterOS 6, 7: настройка актива; Segments=System.Object[]; Index=385}.Id])
> @{Id=7366061707; ReuseId=7394892555; Title=MikroTik RouterOS 6, 7: настройка актива; Depth=3; Path=Справочник по настройке активов / Сетевые устройства / MikroTik RouterOS 6, 7: настройка актива; Segments=System.Object[]; Index=385}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7366061707)

---

Настройку актива нужно выполнять от имени учетной записи администратора устройства.

Для проведения аудита на активе нужно создать учетную запись с правами на чтение для доступа MP 10 Collector по протоколу SSH.

**Задача.** Чтобы создать учетную запись для доступа к активу:

1. На узле, с которого производится настройка, запустите терминальный клиент, поддерживающий протокол SSH.
2. Пройдите аутентификацию на активе.
3. Создайте учетную запись для доступа к активу:
   ```
   user add name=<Логин> group=read password=<Пароль>
   ```
4. Разрешите подключение по протоколу SSH с IP-адреса сервера MP 10 Collector:
   ```
   ip service set ssh address=<IP-адрес MP 10 Collector>
   ```
