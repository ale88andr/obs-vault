---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "8105555467"
reuse_id: "9345100683"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/8105555467"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Диагностика и решение проблем / Справочная информация / Добавление исключений в Kaspersky Endpoint Security для БД PostgreSQL"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Добавление исключений в Kaspersky Endpoint Security для БД PostgreSQL

> [!info] Раздел: System.Collections.Hashtable[@{Id=8105555467; ReuseId=9345100683; Title=Добавление исключений в Kaspersky Endpoint Security для БД PostgreSQL; Depth=4; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Справочная информация / Добавление исключений в Kaspersky Endpoint Security для БД PostgreSQL; Segments=System.Object[]; Index=128}.Id])
> @{Id=8105555467; ReuseId=9345100683; Title=Добавление исключений в Kaspersky Endpoint Security для БД PostgreSQL; Depth=4; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Справочная информация / Добавление исключений в Kaspersky Endpoint Security для БД PostgreSQL; Segments=System.Object[]; Index=128}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/8105555467)

---

В процессе проверки на вирусы Kaspersky Endpoint Security может ошибочно помещать в карантин нужные файлы БД PostgreSQL. Вы можете исключить такие файлы из проверки. Для этого необходимо определить путь расположения файлов и добавить его в список исключений Kaspersky Endpoint Security.

**Задача.** Чтобы определить путь расположения файлов,

1. выполните одно из следующих действий:
   - если PostgreSQL установлена в Docker-контейнере, выполните команду:
   ```bash
   docker inspect -f 'Name: {{println .Name}}{{range .Mounts }}Mount: {{println .Source}}{{end}}' $(docker ps -aqf name=postgres)
   ```
   → На экране появится имя контейнера и пути расположения файлов и журналов.
   → Например:
   ```
   Name: /storage-postgres.mc-application.sqlstorage-1
   Mount: /var/lib/deployed-roles/mc-application/sqlstorage-1/data
   Mount: /var/lib/deployed-roles/mc-application/sqlstorage-1/log
   ```
   > [!note] Примечание
   > По умолчанию файлы БД PostgreSQL расположены в каталоге `/data`.
   - если PostgreSQL установлена как служба, выполните команду:
   ```
   ps -auxw | grep -Eo "postgres\s+?-D\s+[^\s]+\s"
   ```
   → На экране появится путь расположения файлов:
   ```
   postgres -D <Путь расположения файлов>
   ```
   > [!note] Примечание
   > Если служба работает под управлением Рatroni, по умолчанию файлы расположены в каталоге `/data/patroni`.

После определения пути расположения файлов PostgreSQL его необходимо добавить в списки исключений (подробнее см. на сайте [support.kaspersky.ru](https://support.kaspersky.ru/)).
