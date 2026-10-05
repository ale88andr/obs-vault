---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "5736995851"
reuse_id: "5737987723"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5736995851"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Восстановление данных из резервной копии / Восстановление пользовательских определений роли RMQ Message Bus"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Восстановление пользовательских определений роли RMQ Message Bus

> [!info] Раздел: System.Collections.Hashtable[@{Id=5736995851; ReuseId=5737987723; Title=Восстановление пользовательских определений роли RMQ Message Bus; Depth=3; Path=Администрирование MaxPatrol VM / Восстановление данных из резервной копии / Восстановление пользовательских определений роли RMQ Message Bus; Segments=System.Object[]; Index=75}.Id])
> @{Id=5736995851; ReuseId=5737987723; Title=Восстановление пользовательских определений роли RMQ Message Bus; Depth=3; Path=Администрирование MaxPatrol VM / Восстановление данных из резервной копии / Восстановление пользовательских определений роли RMQ Message Bus; Segments=System.Object[]; Index=75}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5736995851)

---

Вы можете восстановить пользовательские определения роли RMQ Message Bus из резервной копии в Docker-контейнере роли после обновления до версии 27.0 и выше.

**Задача.** Чтобы восстановить данные из резервной копии,

1. на каждом из серверов с установленной ролью RMQ Message Bus выполните следующие команды:
   ```bash
   docker cp <Путь к каталогу с резервной копией данных роли на соответствующем сервере>/rmq_definitions.json $(docker ps -aqf name=messagebus-rabbitmq):/tmp/
   docker exec -it $(docker ps -aqf name=messagebus-rabbitmq) rabbitmqctl import_definitions /tmp/rmq_definitions.json
   ```
