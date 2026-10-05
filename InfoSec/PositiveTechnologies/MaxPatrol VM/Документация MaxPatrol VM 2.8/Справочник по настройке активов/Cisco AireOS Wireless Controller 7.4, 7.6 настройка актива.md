---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "975364747"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/975364747"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Устройства беспроводной сети / Cisco AireOS Wireless Controller 7.4, 7.6: настройка актива"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Cisco AireOS Wireless Controller 7.4, 7.6: настройка актива

> [!info] Раздел: System.Collections.Hashtable[@{Id=975364747; ReuseId=; Title=Cisco AireOS Wireless Controller 7.4, 7.6: настройка актива; Depth=3; Path=Справочник по настройке активов / Устройства беспроводной сети / Cisco AireOS Wireless Controller 7.4, 7.6: настройка актива; Segments=System.Object[]; Index=493}.Id])
> @{Id=975364747; ReuseId=; Title=Cisco AireOS Wireless Controller 7.4, 7.6: настройка актива; Depth=3; Path=Справочник по настройке активов / Устройства беспроводной сети / Cisco AireOS Wireless Controller 7.4, 7.6: настройка актива; Segments=System.Object[]; Index=493}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/975364747)

---

Настройку актива нужно выполнять от имени учетной записи с правом перехода в режим глобальной конфигурации.

> [!warning] Внимание
> При использовании в IT-инфраструктуре организации межсетевого экрана или других средств для контроля сетевого трафика требуется настроить в них правила, разрешающие трафик в обоих направлениях между узлом актива и узлом MP 10 Collector. Для проведения аудита актива по протоколу SSH используется порт TCP 22.

Для проведения аудита на активе нужно создать учетную запись для доступа MP 10 Collector по протоколу SSH.

**Задача.** Чтобы создать учетную запись для доступа к активу:

1. На узле, с которого производится настройка актива, запустите терминальный клиент, поддерживающий сетевые протоколы SSH и Telnet.
2. Пройдите аутентификацию на активе.
3. Создайте учетную запись для доступа к активу:
   ```
   config mgmtuser add <Логин> <Пароль> read-only
   ```
4. Сохраните изменения:
   ```
   save config
   ```

> Учетная запись создана.
