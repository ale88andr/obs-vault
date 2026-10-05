---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "3568059275"
reuse_id: "3664779019"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/3568059275"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Смена паролей служебных учетных записей / Смена паролей служебных учетных записей в RabbitMQ / RabbitMQ: смена пароля служебной учетной записи компонента MP 10 Core на Linux"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# RabbitMQ: смена пароля служебной учетной записи компонента MP 10 Core на Linux

> [!info] Раздел: System.Collections.Hashtable[@{Id=3568059275; ReuseId=3664779019; Title=RabbitMQ: смена пароля служебной учетной записи компонента MP 10 Core на Linux; Depth=4; Path=Администрирование MaxPatrol VM / Смена паролей служебных учетных записей / Смена паролей служебных учетных записей в RabbitMQ / RabbitMQ: смена пароля служебной учетной записи компонента MP 10 Core на Linux; Segments=System.Object[]; Index=77}.Id])
> @{Id=3568059275; ReuseId=3664779019; Title=RabbitMQ: смена пароля служебной учетной записи компонента MP 10 Core на Linux; Depth=4; Path=Администрирование MaxPatrol VM / Смена паролей служебных учетных записей / Смена паролей служебных учетных записей в RabbitMQ / RabbitMQ: смена пароля служебной учетной записи компонента MP 10 Core на Linux; Segments=System.Object[]; Index=77}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/3568059275)

---

По умолчанию логин служебной учетной записи компонента MP 10 Core на Linux — `core`, пароль — `P@ssw0rd`.

**Задача.** Чтобы сменить пароль служебной учетной записи MP 10 Core:

1. На сервере MP 10 Core [[Изменение конфигурации роли|измените конфигурации]] ролей Core и RMQ Message Bus:
   ```
   RMQPassword: <Новый пароль>
   ```
2. Перезапустите Docker-контейнер роли RMQ Message Bus:
   ```bash
   cd /var/lib/deployed-roles/<Идентификатор приложения MaxPatrol 10>/<Название экземпляра роли RMQ Message Bus>/images/messagebus-rabbitmq
   docker-compose down
   docker-compose up -d
   ```

> Пароль изменен.
