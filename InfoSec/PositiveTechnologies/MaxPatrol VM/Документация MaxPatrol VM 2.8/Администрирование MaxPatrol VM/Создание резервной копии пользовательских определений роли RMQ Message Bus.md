---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "5736602123"
reuse_id: "5736989835"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5736602123"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Резервное копирование данных / Создание резервной копии пользовательских определений роли RMQ Message Bus"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Создание резервной копии пользовательских определений роли RMQ Message Bus

> [!info] Раздел: System.Collections.Hashtable[@{Id=5736602123; ReuseId=5736989835; Title=Создание резервной копии пользовательских определений роли RMQ Message Bus; Depth=3; Path=Администрирование MaxPatrol VM / Резервное копирование данных / Создание резервной копии пользовательских определений роли RMQ Message Bus; Segments=System.Object[]; Index=73}.Id])
> @{Id=5736602123; ReuseId=5736989835; Title=Создание резервной копии пользовательских определений роли RMQ Message Bus; Depth=3; Path=Администрирование MaxPatrol VM / Резервное копирование данных / Создание резервной копии пользовательских определений роли RMQ Message Bus; Segments=System.Object[]; Index=73}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5736602123)

---

Резервная копия пользовательских определений создается в Docker-контейнере роли RMQ Message Bus.

**Задача.** Чтобы создать резервную копию,

1. на каждом из серверов с установленной ролью RMQ Message Bus выполните следующие команды:
   ```bash
   docker exec -it $(docker ps -aqf name=messagebus-rabbitmq) rabbitmqctl export_definitions /tmp/rmq_definitions.json
   docker cp $(docker ps -aqf name=messagebus-rabbitmq):/tmp/rmq_definitions.json <Путь к каталогу с резервной копией данных роли на соответствующем сервере>/
   ```

Все пользовательские определения роли будут сохранены в файл `<Путь к каталогу с резервной копией данных роли на соответствующем сервере>/rmq_definitions.json`.
