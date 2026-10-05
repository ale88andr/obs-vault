---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "7408460939"
reuse_id: "7713577867"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7408460939"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Диагностика и решение проблем / Большое количество запросов к DNS-серверу от Docker-контейнеров сервера MP 10 Core"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Большое количество запросов к DNS-серверу от Docker-контейнеров сервера MP 10 Core

> [!info] Раздел: System.Collections.Hashtable[@{Id=7408460939; ReuseId=7713577867; Title=Большое количество запросов к DNS-серверу от Docker-контейнеров сервера MP 10 Core; Depth=3; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Большое количество запросов к DNS-серверу от Docker-контейнеров сервера MP 10 Core; Segments=System.Object[]; Index=110}.Id])
> @{Id=7408460939; ReuseId=7713577867; Title=Большое количество запросов к DNS-серверу от Docker-контейнеров сервера MP 10 Core; Depth=3; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Большое количество запросов к DNS-серверу от Docker-контейнеров сервера MP 10 Core; Segments=System.Object[]; Index=110}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7408460939)

---

## Проблема

Повышение нагрузки на DNS-сервер из-за большого количества запросов от Docker-контейнеров сервера MP 10 Core.

## Решение

Необходимо внести изменения в конфигурационный файл `/var/lib/deployer/role_packages/docker/docker-compose.common.yaml` на сервере с установленной ролью Deployer. Перед выполнением инструкции рекомендуется создать резервную копию файла.

**Задача.** Чтобы решить проблему:

1. Откройте файл `/var/lib/deployer/role_packages/docker/docker-compose.common.yaml`.
2. Раскомментируйте строки:
   ```
   #    extra_hosts:
   #      - "fqdn:ip"
   ```
3. Вместо элемента `fqdn:ip` добавьте строки для всех серверов инсталляции в формате <FQDN сервера>:<IP-адрес сервера>.
   > [!warning] Внимание
   > При редактировании файла необходимо соблюдать синтаксис языка YAML.
   → Например:
   ```
   extra_hosts:
   - "core.test.example:10.10.10.10"
   - "siem.test.example:10.10.10.120"
   ```
4. Запустите обновление конфигураций установленных компонентов MaxPatrol VM:
   ```bash
   deployer instance reconfigure '*'
   ```

В результате выполнения команды в конфигурационные файлы `docker-compose.common.yaml` всех ролей будет добавлена информация о серверах инсталляции.

> [!warning] Внимание
> После обновления роли Deployer потребуется заново внести изменения в файл.
