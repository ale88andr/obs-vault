---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Начало-работы-с-MaxPatrol-VM"
doc_id: "8821119243"
reuse_id: "8821765387"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/8821119243"
section: "Начало работы с MaxPatrol VM"
breadcrumb: "Начало работы с MaxPatrol VM / Оптимизация работы MaxPatrol VM"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Оптимизация работы MaxPatrol VM

> [!info] Раздел: System.Collections.Hashtable[@{Id=8821119243; ReuseId=8821765387; Title=Оптимизация работы MaxPatrol VM; Depth=2; Path=Начало работы с MaxPatrol VM / Оптимизация работы MaxPatrol VM; Segments=System.Object[]; Index=164}.Id])
> @{Id=8821119243; ReuseId=8821765387; Title=Оптимизация работы MaxPatrol VM; Depth=2; Path=Начало работы с MaxPatrol VM / Оптимизация работы MaxPatrol VM; Segments=System.Object[]; Index=164}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/8821119243)

---

Для повышения быстродействия и надежности MaxPatrol VM рекомендуется изменить значения параметров по умолчанию для ряда компонентов.

## Сервис TRM

Рекомендуется увеличить таймаут транзакций базы данных.

**Задача.** Чтобы настроить таймаут транзакций:

1. На сервере с установленной ролью Core перейдите в каталог `/var/lib/deployed-roles/<Название приложения>/<Название роли Core>/images/core.assets.temporalreadmodel.extended/config/`.
   → Пример:
   ```bash
   cd /var/lib/deployed-roles/mp10-application/core/images/core.assets.temporalreadmodel.extended/config/
   ```
2. Откройте для редактирования файл `custom.env` и добавьте строку:
   ```
   Database_TransactionTimeout=06:00:00
   ```
3. Перезапустите Docker-контейнер:
   ```bash
   docker-compose down && docker-compose up -d
   ```

## Сервис групп активов

Рекомендуется увеличить интервал пересчета динамических групп и отключить пересчет при добавлении активов.

**Задача.** Чтобы оптимизировать пересчет динамических групп:

1. На сервере с установленной ролью Core перейдите в каталог `/var/lib/deployed-roles/<Название приложения>/<Название роли Core>/images/core.assets.groups.extended/config/`.
   → Пример:
   ```bash
   cd /var/lib/deployed-roles/mp10-application/core/images/core.assets.groups.extended/config/
   ```
2. Откройте для редактирования файл `custom.env` и добавьте строки:
   ```
   GroupsScheduler_RecalculationTimeout=03:00:00
   GroupsScheduler_ForceRecalculateOnAssetCreate=false
   ```
3. Перезапустите Docker-контейнер:
   ```bash
   docker-compose down && docker-compose up -d
   ```

## Сервис виджетов

Рекомендуется сократить длину очереди на прогрев кэша.

**Задача.** Чтобы сократить очередь:

1. На сервере с установленной ролью Core перейдите в каталог `/var/lib/deployed-roles/<Название приложения>/<Название роли Core>/images/core.analytics.widgets.extended/config/`.
   → Пример:
   ```bash
   cd /var/lib/deployed-roles/mp10-application/core/images/ core.analytics.widgets.extended/config/
   ```
2. Откройте для редактирования файл `custom.env` и добавьте строку:
   ```
   CacheWarmerSettings_AssetsWarmingQueueLimit=4
   ```
3. Перезапустите Docker-контейнер:
   ```bash
   docker-compose down && docker-compose up -d
   ```

## Политики обновления дашбордов

**Задача.** Чтобы настроить периодичность обновления для виджета на дашборде:

1. На виджете нажмите  и выберите **Настроить**.
2. Выберите вариант обновления.

**Задача.** Чтобы настроить периодичность обновления для нескольких виджетов:

1. На сервере с установленной ролью SqlStorage выполните команду:
   ```bash
   deployer instance configure -type sqlstorage
   ```
2. Выберите установленное приложение.
3. Выберите установленный экземпляр роли.
4. Выберите **Advanced configuration**.
5. Используйте значения параметров `PgAdminPort`, `PgUser` и `PgPassword`, чтобы подключиться к серверу PostgreSQL на том же узле.
6. В БД `maxpatrol_analiticswidgets` выполните запрос:
   ```
   UPDATE widgets
   SET widget_object=jsonb_set(
   widget_object::jsonb,
   '{refreshCachingPolicy}',
   jsonb '{"type":"<Вариант обновления>"[,"interval":{"value":"<Интервал в минутах>mi"}}']
   )
   WHERE
   widgets_container_id = <ID дашборда>
   [AND widget_object::json#>>'{source,0}' = '<Тип виджетов>']
   ```

В этом запросе `<Вариант обновления>` может принимать значения:

- `forever` — только вручную;
- `never` — при открытии дашборда;
- `periodical` — периодически.

`<Тип виджетов>` может принимать значения:

- `assets` — виджеты по активам и уязвимостям;
- `events` — виджеты по событиям.

**Задача.** Чтобы определить ID дашборда:

1. Откройте дашборд.
2. В адресной строке браузера скопируйте значение параметра `dashboardId`.
   → Например, ID дашборда **Управление уязвимостями** — 11.

## Сервер PostgreSQL

Рекомендуется настроить параметры использования памяти сервером PostgreSQL, чтобы повысить быстродействие и сократить вероятность ошибок переполнения памяти.

**Задача.** Чтобы настроить параметры буферов и кэша сервера PostgreSQL:

1. На сервере с установленной ролью SqlStorage выполните команду:
   ```bash
   deployer instance configure -type sqlstorage
   ```
2. Выберите установленное приложение.
3. Выберите установленный экземпляр роли.
4. Выберите **Advanced configuration**.
5. Укажите рекомендованные значения для параметров `PgEffectiveCacheSize`, `PgSharedBufferSize` и `PgWorkMem`.

**Рекомендуемые параметры сервера PostgreSQL**

| Объем ОЗУ | PgEffectiveCacheSize | PgSharedBufferSize | PgWorkMem |
| --- | --- | --- | --- |
| 64 ГБ | 25GB | 20GB | 200MB |
| 96 ГБ | 36GB | 30GB | 384MB |
| 128 ГБ | 50GB | 40GB | 512MB |
| 160 ГБ | 62GB | 50GB | 640MB |
| 192 ГБ | 75GB | 60GB | 768MB |
| 224 ГБ | 87GB | 70GB | 896MB |
| 256 ГБ | 100GB | 80GB | 1024MB |

Подробности см. [в документации PostgreSQL](https://postgrespro.ru/docs/postgresql/17/).
