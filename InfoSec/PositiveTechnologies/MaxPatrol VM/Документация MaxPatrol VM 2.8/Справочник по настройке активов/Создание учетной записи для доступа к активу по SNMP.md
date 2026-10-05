---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "987070859"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/987070859"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Сетевые устройства / Alcatel OmniSwitch 6.6.4: настройка актива / Создание учетной записи для доступа к активу по SNMP"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание учетной записи для доступа к активу по SNMP

> [!info] Раздел: System.Collections.Hashtable[@{Id=987070859; ReuseId=; Title=Создание учетной записи для доступа к активу по SNMP; Depth=4; Path=Справочник по настройке активов / Сетевые устройства / Alcatel OmniSwitch 6.6.4: настройка актива / Создание учетной записи для доступа к активу по SNMP; Segments=System.Object[]; Index=323}.Id])
> @{Id=987070859; ReuseId=; Title=Создание учетной записи для доступа к активу по SNMP; Depth=4; Path=Справочник по настройке активов / Сетевые устройства / Alcatel OmniSwitch 6.6.4: настройка актива / Создание учетной записи для доступа к активу по SNMP; Segments=System.Object[]; Index=323}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/987070859)

---

**Задача.** Чтобы создать учетную запись для доступа к активу:

1. На узле, с которого производится настройка актива, запустите терминальный клиент, поддерживающий сетевые протоколы SSH и Telnet.
2. Пройдите аутентификацию на активе.
3. Создайте учетную запись для доступа к активу:
   ```
   user <Логин> password <Пароль> read-write all no auth
   ```
4. Настройте сервис SNMP:
   ```
   security no security
   snmp community map "<Название группы>" user "<Логин>" on
   ```
5. Разрешите доступ по протоколу SNMP с IP-адрес узла MP 10 Collector:
   ```
   snmp station <IP-адрес MP 10 Collector> 162 enable v2 "<Логин>"
   ```
6. Сохраните изменения:
   ```
   configuration snapshot all
   ```

> Учетная запись создана.
