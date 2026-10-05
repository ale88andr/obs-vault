---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-по-настройке-активов"
doc_id: "10108190347"
reuse_id: "10109292299"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/10108190347"
section: "Справочник по настройке активов"
breadcrumb: "Справочник по настройке активов / Сетевые устройства / Check Point GAiA OS 80.10 и выше: настройка актива / Создание учетной записи для доступа к активу по SSH"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание учетной записи для доступа к активу по SSH

> [!info] Раздел: System.Collections.Hashtable[@{Id=10108190347; ReuseId=10109292299; Title=Создание учетной записи для доступа к активу по SSH; Depth=4; Path=Справочник по настройке активов / Сетевые устройства / Check Point GAiA OS 80.10 и выше: настройка актива / Создание учетной записи для доступа к активу по SSH; Segments=System.Object[]; Index=344}.Id])
> @{Id=10108190347; ReuseId=10109292299; Title=Создание учетной записи для доступа к активу по SSH; Depth=4; Path=Справочник по настройке активов / Сетевые устройства / Check Point GAiA OS 80.10 и выше: настройка актива / Создание учетной записи для доступа к активу по SSH; Segments=System.Object[]; Index=344}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/10108190347)

---

**Задача.** Чтобы создать учетную запись для доступа к активу:

1. На узле, с которого производится настройка актива, запустите терминальный клиент, поддерживающий протокол SSH.
2. Пройдите аутентификацию на активе.
3. Создайте роль для выполнения команды `cpstat`:
   ```
   add rba role cpstat domain-type System readwrite-features ext_cpstat
   ```
4. Создайте учетную запись для доступа к активу:
   ```
   add user <Логин> uid 0 homedir /home/<Логин>
   ```
5. Установите пароль учетной записи:
   ```
   set user <Логин> password
   ```
6. Назначьте пользователю роли monitorRole и cpstat:
   ```
   add rba user <Логин> roles monitorRole,cpstat
   ```
7. Сохраните изменения:
   ```
   save config
   ```
