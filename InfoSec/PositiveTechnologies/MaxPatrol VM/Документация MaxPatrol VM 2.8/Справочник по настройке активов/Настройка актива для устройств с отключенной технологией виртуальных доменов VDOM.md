---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "6273058187"
reuse_id: "7856808971"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6273058187"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Межсетевые экраны / Fortinet FortiGate 5.4.2—7.4.4: настройка актива / Настройка актива для устройств с отключенной технологией виртуальных доменов VDOM"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Настройка актива для устройств с отключенной технологией виртуальных доменов VDOM

> [!info] Раздел: System.Collections.Hashtable[@{Id=6273058187; ReuseId=7856808971; Title=Настройка актива для устройств с отключенной технологией виртуальных доменов VDOM; Depth=4; Path=Справочник по настройке активов / Межсетевые экраны / Fortinet FortiGate 5.4.2—7.4.4: настройка актива / Настройка актива для устройств с отключенной технологией виртуальных доменов VDOM; Segments=System.Object[]; Index=303}.Id])
> @{Id=6273058187; ReuseId=7856808971; Title=Настройка актива для устройств с отключенной технологией виртуальных доменов VDOM; Depth=4; Path=Справочник по настройке активов / Межсетевые экраны / Fortinet FortiGate 5.4.2—7.4.4: настройка актива / Настройка актива для устройств с отключенной технологией виртуальных доменов VDOM; Segments=System.Object[]; Index=303}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6273058187)

---

## Для FortiGate 6.4.0 и более новых версий

**Задача.** Чтобы создать учетную запись для доступа к активу:

1. На узле, с которого производится настройка актива, запустите терминальный клиент, поддерживающий сетевые протоколы SSH и Telnet.
2. Пройдите аутентификацию на активе.
3. Перейдите в режим конфигурирования:
   ```
   config system admin
   ```
4. Создайте учетную запись для доступа к активу:
   ```
   edit "<Логин>"
   set accprofile "super_admin_readonly"
   set vdom "root"
   set password "<Пароль>"
   ```
5. Выйдите из режима конфигурирования:
   ```
   end
   ```

## Для FortiGate 5.4.2—6.2.17

**Задача.** Чтобы создать учетную запись для доступа к активу:

1. На узле, с которого производится настройка актива, запустите терминальный клиент, поддерживающий сетевые протоколы SSH и Telnet.
2. Пройдите аутентификацию на активе.
3. Перейдите в режим создания профиля:
   ```
   config system accprofile
   ```
4. Создайте профиль с правами на чтение и запись:
   ```
   edit "<"Профиль">"
   set mntgrp read-write
   set admingrp read-write
   set updategrp read-write
   set authgrp read-write
   set sysgrp read-write
   set netgrp read-write
   set loggrp read-write
   set routegrp read-write
   set fwgrp read-write
   set vpngrp read-write
   set utmgrp read-write
   set wanoptgrp read-write
   set endpoint-control-grp read-write
   set wifi read-write
   ```
5. Выйдите из режима создания профиля:
   ```
   end
   ```
6. Перейдите в режим создания учетной записи:
   ```
   config system admin
   ```
7. Создайте учетную запись для доступа к активу:
   ```
   edit "<Логин>"
   set accprofile "<Профиль>"
   set password "<Пароль>"
   ```
8. Выйдите из режима создания учетной записи:
   ```
   end
   ```
