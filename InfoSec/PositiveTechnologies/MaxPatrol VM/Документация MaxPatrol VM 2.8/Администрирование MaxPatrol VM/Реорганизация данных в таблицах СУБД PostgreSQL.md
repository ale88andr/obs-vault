---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "7872735243"
reuse_id: "9345008779"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7872735243"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Диагностика и решение проблем / Справочная информация / Реорганизация данных в таблицах СУБД PostgreSQL"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Реорганизация данных в таблицах СУБД PostgreSQL

> [!info] Раздел: System.Collections.Hashtable[@{Id=7872735243; ReuseId=9345008779; Title=Реорганизация данных в таблицах СУБД PostgreSQL; Depth=4; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Справочная информация / Реорганизация данных в таблицах СУБД PostgreSQL; Segments=System.Object[]; Index=126}.Id])
> @{Id=7872735243; ReuseId=9345008779; Title=Реорганизация данных в таблицах СУБД PostgreSQL; Depth=4; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Справочная информация / Реорганизация данных в таблицах СУБД PostgreSQL; Segments=System.Object[]; Index=126}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7872735243)

---

При работе с СУБД PostgreSQL данные таблиц БД могут фрагментироваться, что приводит к ухудшению производительности работы системы и нерациональному использованию дискового пространства. Чтобы безопасно реорганизовать фрагментированные данные таблиц, восстановить индексы и вернуть дисковое пространство без влияния на производительность БД, можно использовать утилиту pgcompacttable, которая поставляется с ролью SqlStorage.

Утилита pgcompacttable является альтернативой команды `VACUUM FULL`, но, в отличие от `VACUUM FULL`, не требует большого количества свободного места на диске и не блокирует таблицы на время своей работы. В ходе работы утилиты таблицы обрабатываются с адаптивными задержками для предотвращения перегрузки ввода и вывода данных и задержек репликации.

> [!warning] Внимание
> Не запускайте утилиту pgcompacttable и команду `VACUUM FULL` одновременно.

Для работы утилиты pgcompacttable необходимо свободное место, равное размеру самого большого индекса в БД. Перед запуском утилиты для каждой БД, где планируется использовать утилиту, необходимо создать расширение `pgstattuple` с помощью команды `docker exec -it $(docker ps | awk '/storage-postgres/ && !/EDR/ {print $NF}') psql -U pt_system -d <Имя БД PostgreSQL> -c "CREATE EXTENSION IF NOT EXISTS pgstattuple;"`.

Узнать размер БД для сравнения результатов до и после работы утилиты вы можете с помощью команды `docker exec -it $(docker ps | awk '/storage-postgres/ && !/EDR/ {print $NF}') psql -c "SELECT datname AS Database_Name, pg_size_pretty(pg_database_size(datname)) AS Size FROM pg_database ORDER BY pg_database_size(datname) DESC LIMIT 15;" -d postgres -U pt_system`.

Продолжительность работы утилиты зависит от размера БД PostgreSQL, объема фрагментированных данных, а также от аппаратного обеспечения сервера и нагрузки на него, и в некоторых случаях может занимать до нескольких часов. Чтобы работа утилиты не прервалась из-за разрыва соединения, рекомендуется выполнять запуск в одном из терминальных мультиплексоров, например в screen.

**Задача.** Чтобы запустить утилиту,

1. на сервере с установленной ролью SqlStorage выполните команду:
   ```bash
   docker exec -it $(docker ps | awk '/storage-postgres/ && !/EDR/ {print $NF}') pgcompacttable --verbose -U pt_system -d <Имя БД PostgreSQL> >> /var/tmp/pgcompacttable.log
   ```

Журнал работы утилиты будет сохранен в файл `/var/tmp/pgcompacttable.log`.

При запуске утилиты вы можете использовать специальные ключи:

- `-a` и `--all`

  Запуск утилиты на всех БД кластера.
- `-t TABLE` и `--table TABLE`

  Запуск утилиты для конкретной таблицы.
- `-T TABLE` и `--exclude-table TABLE`

  Исключение таблицы из обработки.
- `-n SCHEMA` и `--schema SCHEMA`

  Обработка таблиц и индексов в конкретной схеме.
- `- N SCHEMA` и `--exclude-schema SCHEMA`

  Исключение схемы из обработки.
- `-f` и `--force`

  Принудительный запуск обработки.

  > [!note] Примечание
  > Без использования этого ключа таблицы и индексы с избыточностью данных менее 20% по умолчанию не обрабатываются.
- `--man`

  Просмотр списка всех доступных ключей и информации о них.

Пример запуска утилиты на всех БД кластера:

`docker exec -it $(docker ps | awk '/storage-postgres/ && !/EDR/ {print $NF}') pgcompacttable --all --verbose -U pt_system >> /var/tmp/pgcompacttable.log`
