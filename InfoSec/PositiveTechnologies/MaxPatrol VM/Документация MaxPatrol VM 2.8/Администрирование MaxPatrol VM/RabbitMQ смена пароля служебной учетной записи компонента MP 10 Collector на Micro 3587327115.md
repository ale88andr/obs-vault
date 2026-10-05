---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "3587327115"
reuse_id: "3664780555"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/3587327115"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Смена паролей служебных учетных записей / Смена паролей служебных учетных записей в RabbitMQ / RabbitMQ: смена пароля служебной учетной записи компонента MP 10 Collector на Microsoft Windows"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# RabbitMQ: смена пароля служебной учетной записи компонента MP 10 Collector на Microsoft Windows

> [!info] Раздел: System.Collections.Hashtable[@{Id=3587327115; ReuseId=3664780555; Title=RabbitMQ: смена пароля служебной учетной записи компонента MP 10 Collector на Microsoft Windows; Depth=4; Path=Администрирование MaxPatrol VM / Смена паролей служебных учетных записей / Смена паролей служебных учетных записей в RabbitMQ / RabbitMQ: смена пароля служебной учетной записи компонента MP 10 Collector на Microsoft Windows; Segments=System.Object[]; Index=78}.Id])
> @{Id=3587327115; ReuseId=3664780555; Title=RabbitMQ: смена пароля служебной учетной записи компонента MP 10 Collector на Microsoft Windows; Depth=4; Path=Администрирование MaxPatrol VM / Смена паролей служебных учетных записей / Смена паролей служебных учетных записей в RabbitMQ / RabbitMQ: смена пароля служебной учетной записи компонента MP 10 Collector на Microsoft Windows; Segments=System.Object[]; Index=78}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/3587327115)

---

По умолчанию логин служебной учетной записи компонента MP 10 Collector на Microsoft Windows для доступа к RabbitMQ — `mpx_agent`, пароль — `P@ssw0rd`.

**Задача.** Чтобы сменить пароль служебной учетной записи для доступа к RabbitMQ, установленному на сервер MP 10 Core под управлением Linux:

1. Измените конфигурацию ролей RMQ Message Bus и Core, установленных на сервере MP 10 Core:
   ```
   RMQAgentPassword: <Новый пароль>
   ```
2. Перезапустите Docker-контейнер роли RMQ Message Bus:
   ```bash
   cd /var/lib/deployed-roles/<Идентификатор приложения MaxPatrol 10>/<Название экземпляра роли RMQ Message Bus>/images/messagebus-rabbitmq
   docker-compose down
   docker-compose up -d
   ```
3. На сервере каждого компонента MP 10 Collector выполните команду:
   ```
   coreagentcfg set -p RMQUser agent RMQPassword <Новый пароль>
   ```

> Пароль изменен.
