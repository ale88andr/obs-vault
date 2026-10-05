---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "1178625291"
reuse_id: "5860506891"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/1178625291"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Отправка уведомлений через POST-запрос / Поля POST-запроса для уведомления"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Поля POST-запроса для уведомления

> [!info] Раздел: System.Collections.Hashtable[@{Id=1178625291; ReuseId=5860506891; Title=Поля POST-запроса для уведомления; Depth=3; Path=Справочник разработчика / Отправка уведомлений через POST-запрос / Поля POST-запроса для уведомления; Segments=System.Object[]; Index=735}.Id])
> @{Id=1178625291; ReuseId=5860506891; Title=Поля POST-запроса для уведомления; Depth=3; Path=Справочник разработчика / Отправка уведомлений через POST-запрос / Поля POST-запроса для уведомления; Segments=System.Object[]; Index=735}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/1178625291)

---

MaxPatrol VM отправляет POST-запрос в формате JSON. В зависимости от типа запрос может содержать следующие поля:

- `message_id` — идентификатор POST-запроса.
- `notification_type` — тип POST-запроса: `test` — тестовый запрос, `event` — для мгновенного уведомления, `schedule` — для уведомления за период времени.
- `notification_source` — условие создания уведомления: `AssetStateMetaTrigger` — изменение состава активов в системе; `EventsMetaTrigger` — получение события, удовлетворяющего выбранному фильтру; `EventsMonitoringControlsMetaTrigger` — выход параметров потока событий от источника из выбранной группы за пределы допустимых значений; `GroupContentMetaTrigger` — изменение состава активов в выбранной группе; `HealthMonitoringIssuesMetaTrigger` — появление уведомления о состоянии системы; `ScannerTaskNotificationsMetaTrigger` — запуск или остановка задачи на сбор данных.
- `notification_uid` — идентификатор уведомления.
- `notification_name` — название задачи MaxPatrol VM на отправку уведомления.
- `uri` — ссылка на данные уведомления.

  > [!warning] Внимание
  > Данные уведомления доступны по ссылке в течение 24 часов с момента создания уведомления.
- `schema_uri` — ссылка на схему данных уведомления в формате JSON.
- `event_time_stamp` — дата и время создания мгновенного уведомления (UTC+0).
- `time_interval` — для уведомления за период содержит поля `event_time_start` и `event_time_end` с датами и временем начала и конца периода (UTC+0).
- `time_stamp` — дата и время создания POST-запроса (UTC+0).

Пример тестового POST-запроса:

```json
{
  "notification_type":"test",
  "notification_source":"AssetStateMetaTrigger",
  "schema_uri":"https://vm-server.ru:8733/api/assets_triggers/v1/triggers_data/meta_triggers/AssetStateMetaTrigger/reactions/webhook_notification/schema",
  "time_stamp":"2019-04-15T11:20:49.7031727Z"
}
```

Пример POST-запроса для уведомления за период времени:

```json
{
  "message_id":1194,
  "notification_type":"schedule",
  "notification_source":"AssetStateMetaTrigger",
  "notification_uid":"110a1602-d980-0001-0000-000000000006",
  "notification_name":"1",
"uri":"https://vm-server.ru:8733/api/assets_triggers/v1/triggers_data/meta_triggers/AssetStateMetaTrigger/reactions/webhook_notification/110a718e730000010000000000000261",
  "schema_uri":"https://vm-server.ru:8733/api/assets_triggers/v1/triggers_data/meta_triggers/AssetStateMetaTrigger/reactions/webhook_notification/schema",
  "event_time_stamp":"2019-04-12T09:50:21.4309566Z",
  "time_interval": {
    "event_time_start": "2019-04-12T09:50:21.4309566Z",
    "event_time_end": "2019-04-12T09:55:21.4309566Z"
  },
  "time_stamp":"2019-04-12T09:55:33.0096243Z"
}
```
