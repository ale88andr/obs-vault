---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "980892939"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/980892939"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Сетевые устройства / Juniper JunOS 11—19: настройка актива / Создание пароля для доступа к активу по SNMP"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание пароля для доступа к активу по SNMP

> [!info] Раздел: System.Collections.Hashtable[@{Id=980892939; ReuseId=; Title=Создание пароля для доступа к активу по SNMP; Depth=4; Path=Справочник по настройке активов / Сетевые устройства / Juniper JunOS 11—19: настройка актива / Создание пароля для доступа к активу по SNMP; Segments=System.Object[]; Index=381}.Id])
> @{Id=980892939; ReuseId=; Title=Создание пароля для доступа к активу по SNMP; Depth=4; Path=Справочник по настройке активов / Сетевые устройства / Juniper JunOS 11—19: настройка актива / Создание пароля для доступа к активу по SNMP; Segments=System.Object[]; Index=381}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/980892939)

---

**Задача.** Чтобы создать пароль для доступа к активу:

1. На узле, с которого производится настройка актива, запустите терминальный клиент, поддерживающий сетевые протоколы SSH и Telnet.
2. Пройдите аутентификацию на активе.
3. Перейдите в режим конфигурирования:
   ```
   configure
   ```
4. Создайте пароль для доступа к активу:
   ```
   edit snmp community <Пароль>
   ```
5. Настройте доступ с IP-адреса узла MP 10 Collector с правом на чтение:
   ```
   set clients <IP-адрес MP 10 Collector>/32
   set authorization read-only
   ```
6. Примените изменения и выйдите из режима конфигурирования:
   ```
   end
   ```

> Пароль создан.
