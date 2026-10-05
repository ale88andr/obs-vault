---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "6272658827"
reuse_id: "7856809739"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6272658827"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Межсетевые экраны / Fortinet FortiGate 5.4.2—7.4.4: настройка актива / Настройка актива для устройств с включенной технологией виртуальных доменов VDOM"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Настройка актива для устройств с включенной технологией виртуальных доменов VDOM

> [!info] Раздел: System.Collections.Hashtable[@{Id=6272658827; ReuseId=7856809739; Title=Настройка актива для устройств с включенной технологией виртуальных доменов VDOM; Depth=4; Path=Справочник по настройке активов / Межсетевые экраны / Fortinet FortiGate 5.4.2—7.4.4: настройка актива / Настройка актива для устройств с включенной технологией виртуальных доменов VDOM; Segments=System.Object[]; Index=304}.Id])
> @{Id=6272658827; ReuseId=7856809739; Title=Настройка актива для устройств с включенной технологией виртуальных доменов VDOM; Depth=4; Path=Справочник по настройке активов / Межсетевые экраны / Fortinet FortiGate 5.4.2—7.4.4: настройка актива / Настройка актива для устройств с включенной технологией виртуальных доменов VDOM; Segments=System.Object[]; Index=304}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6272658827)

---

**Задача.** Чтобы создать учетную запись для доступа к активу:

1. На узле, с которого производится настройка актива, запустите терминальный клиент, поддерживающий сетевые протоколы SSH и Telnet.
2. Пройдите аутентификацию на активе.
3. Перейдите в режим глобального конфигурирования:
   ```
   config global
   ```
4. Перейдите в режим конфигурирования системных профилей:
   ```
   config system accprofile
   ```
5. Создайте профиль:
   ```
   edit "<Название профиля>"
   ```
   - Для FortiGate 6.4.0 и более новых версий с правом на чтение:
   ```
   set scope vdom
   set secfabgrp read
   set ftviewgrp read
   set authgrp read
   set sysgrp read
   set netgrp read
   set loggrp read
   set fwgrp read
   set vpngrp read
   set utmgrp read
   set wifi read
   ```
   - Для FortiGate 5.4.2—6.2.17 с правом на чтение и запись:
   ```
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
   set scope vdom
   ```
6. Выйдите из режима глобального конфигурирования:
   ```
   end
   ```
7. Перейдите в режим конфигурирования учетных записей:
   ```
   config system admin
   ```
8. Создайте для каждого виртуального домена учетную запись для доступа к активу:
   ```
   edit "<Логин>"
   set accprofile "<Название профиля>"
   set vdom "<Имя виртуального домена>"
   set password "<Пароль>"
   ```
   → Например:
   ```
   edit "username1_example"
   set accprofile "RO_Profile_example"
   set vdom "vdom1_example"
   set password "password1_example"
   next
   edit "username2_example"
   set accprofile "RO_Profile_example"
   set vdom "vdom2_example"
   set password "password2_example"
   ```
9. Выйдите из режима конфигурирования:
   ```
   end
   ```
10. Выйдите из режима глобального конфигурирования:
    ```
    end
    ```
