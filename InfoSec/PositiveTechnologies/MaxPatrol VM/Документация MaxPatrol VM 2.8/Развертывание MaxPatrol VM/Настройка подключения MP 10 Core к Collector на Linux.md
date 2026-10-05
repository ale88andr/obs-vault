---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Развертывание-MaxPatrol-VM"
doc_id: "11189214091"
reuse_id: "11189788939"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/11189214091"
section: "Развертывание MaxPatrol VM"
breadcrumb: "Развертывание MaxPatrol VM / Настройка подключения к Collector, расположенному в недоверенном сегменте / Настройка подключения MP 10 Core к Collector на Linux"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Настройка подключения MP 10 Core к Collector на Linux

> [!info] Раздел: System.Collections.Hashtable[@{Id=11189214091; ReuseId=11189788939; Title=Настройка подключения MP 10 Core к Collector на Linux; Depth=3; Path=Развертывание MaxPatrol VM / Настройка подключения к Collector, расположенному в недоверенном сегменте / Настройка подключения MP 10 Core к Collector на Linux; Segments=System.Object[]; Index=23}.Id])
> @{Id=11189214091; ReuseId=11189788939; Title=Настройка подключения MP 10 Core к Collector на Linux; Depth=3; Path=Развертывание MaxPatrol VM / Настройка подключения к Collector, расположенному в недоверенном сегменте / Настройка подключения MP 10 Core к Collector на Linux; Segments=System.Object[]; Index=23}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/11189214091)

---

После установки ролей Collector и RMQ Message Bus необходимо настроить подключение MP 10 Core к Collector. Для этого необходимо настроить плагин Shovel для отправки сообщений из очереди RabbitMQ.

## Настройка плагина Shovel для отправки сообщений из очереди

В командах ниже используются следующие переменные, которые перед выполнением команд нужно заменить на реальные значения из вашей инфраструктуры, убрав треугольные скобки:

- <COLLECTOR_NAME> — произвольное название коллектора.
- <MP10_COLLECTOR_ADDRESS> — IP-адрес или FQDN коллектора.
- <MP10_CORE_ADDRESS> — IP-адрес или FQDN сервера MP 10 Core.

**Задача.** Чтобы настроить плагин:

1. На узле, с которого производится настройка плагина, запустите терминальный клиент, поддерживающий сетевой протокол SSH.
2. Подключитесь по протоколу SSH к серверу MP 10 Core.
3. Выполните команду для создания очереди RabbitMQ:
   ```bash
   sudo docker exec -ti $(sudo docker ps -q --filter name=rabbitmq) rabbitmqctl set_parameter -p mpx shovel fwd_agentlinux<COLLECTOR_NAME> '{"src-uri": "amqps://<MP10_CORE_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "dest-uri": "amqps://<MP10_COLLECTOR_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "src-queue": "fwd_agentlinux_<COLLECTOR_NAME>.v2"}'
   ```
   → Будет создана очередь RabbitMQ с названием `fwd_agentlinux_<COLLECTOR_NAME>.v2`.
4. Выполните команду для создания и настройки плагина Shovel:
   ```bash
   sudo docker exec -ti $(sudo docker ps -q --filter name=rabbitmq) rabbitmqctl set_parameter -p mpx shovel agent.<COLLECTOR_NAME>.config '{"src-uri": "amqps://<MP10_COLLECTOR_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "dest-uri": "amqps://<MP10_CORE_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "src-exchange": "pt.mpx.agent.v2", "src-exchange-key": "agent.*.config"}' && sudo docker exec -ti $(sudo docker ps -q --filter name=rabbitmq) rabbitmqctl set_parameter -p mpx shovel agent.<COLLECTOR_NAME>.command_package_ack '{"src-uri": "amqps://<MP10_COLLECTOR_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "dest-uri": "amqps://<MP10_CORE_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "src-exchange": "pt.mpx.agent.v2", "src-exchange-key": "agent.*.command_package_ack"}' && sudo docker exec -ti $(sudo docker ps -q --filter name=rabbitmq) rabbitmqctl set_parameter -p mpx shovel agent.<COLLECTOR_NAME>.event_package '{"src-uri": "amqps://<MP10_COLLECTOR_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "dest-uri": "amqps://<MP10_CORE_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "src-exchange": "pt.mpx.agent.v2", "src-exchange-key": "agent.*.event_package"}' && sudo docker exec -ti $(sudo docker ps -q --filter name=rabbitmq) rabbitmqctl set_parameter -p mpx shovel agent.<COLLECTOR_NAME>.command_package_rejection '{"src-uri": "amqps://<MP10_COLLECTOR_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "dest-uri": "amqps://<MP10_CORE_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "src-exchange": "pt.mpx.agent.v2", "src-exchange-key": "agent.*.command_package_rejection"}' && sudo docker exec -ti $(sudo docker ps -q --filter name=rabbitmq) rabbitmqctl set_parameter -p mpx shovel agent.<COLLECTOR_NAME>.job.artifacts '{"src-uri": "amqps://<MP10_COLLECTOR_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "dest-uri": "amqps://<MP10_CORE_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "src-exchange": "pt.mpx.agent.v2", "src-exchange-key": "agent.*.job.*.artifacts"}' && sudo docker exec -ti $(sudo docker ps -q --filter name=rabbitmq) rabbitmqctl set_parameter -p mpx shovel agent.<COLLECTOR_NAME>.job.progress '{"src-uri": "amqps://<MP10_COLLECTOR_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "dest-uri": "amqps://<MP10_CORE_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "src-exchange": "pt.mpx.agent.v2", "src-exchange-key": "agent.*.job.*.progress"}' && sudo docker exec -ti $(sudo docker ps -q --filter name=rabbitmq) rabbitmqctl set_parameter -p mpx shovel agent.<COLLECTOR_NAME>.job.progress '{"src-uri": "amqps://<MP10_COLLECTOR_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "dest-uri": "amqps://<MP10_CORE_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "src-exchange": "pt.mpx.agent.v2", "src-exchange-key": "agent.*.job.*.result.audit_check"}' && sudo docker exec -ti $(sudo docker ps -q --filter name=rabbitmq) rabbitmqctl set_parameter -p mpx shovel agent.<COLLECTOR_NAME>.job.progress '{"src-uri": "amqps://<MP10_COLLECTOR_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "dest-uri": "amqps://<MP10_CORE_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "src-exchange": "pt.mpx.agent.v2", "src-exchange-key": "agent.*.job.*.result.asset.event"}' && sudo docker exec -ti $(sudo docker ps -q --filter name=rabbitmq) rabbitmqctl set_parameter -p mpx shovel agent.<COLLECTOR_NAME>.job.result.model '{"src-uri": "amqps://<MP10_COLLECTOR_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "dest-uri": "amqps://<MP10_CORE_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "src-exchange": "pt.mpx.agent.v2", "src-exchange-key": "agent.*.job.*.result.model"}' && sudo docker exec -ti $(sudo docker ps -q --filter name=rabbitmq) rabbitmqctl set_parameter -p mpx shovel agent.<COLLECTOR_NAME>.job.savepoint '{"src-uri": "amqps://<MP10_COLLECTOR_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "dest-uri": "amqps://<MP10_CORE_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "src-exchange": "pt.mpx.agent.v2", "src-exchange-key": "agent.*.job.*.savepoint"}' && sudo docker exec -ti $(sudo docker ps -q --filter name=rabbitmq) rabbitmqctl set_parameter -p mpx shovel agent.<COLLECTOR_NAME>.keepalive '{"src-uri": "amqps://<MP10_COLLECTOR_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "dest-uri": "amqps://<MP10_CORE_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "src-exchange": "pt.mpx.agent.v2", "src-exchange-key": "agent.*.keepalive"}' && sudo docker exec -ti $(sudo docker ps -q --filter name=rabbitmq) rabbitmqctl set_parameter -p mpx shovel pt.mpx.incidents.events.v4-<COLLECTOR_NAME> '{"src-uri": "amqps://<MP10_COLLECTOR_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "dest-uri": "amqps://<MP10_CORE_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "src-exchange": "pt.mpx.incidents.events", "src-exchange-key": "pt.mpx.incidents.events.v4"}' && sudo docker exec -ti $(sudo docker ps -q --filter name=rabbitmq) rabbitmqctl set_parameter -p mpx shovel monitoring-<COLLECTOR_NAME> '{"src-uri": "amqps://<MP10_COLLECTOR_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "dest-uri": "amqps://<MP10_CORE_ADDRESS>/mpx?cacertfile=/usr/local/share/rabbitmq/certs/rootCA.crt&certfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.crt&keyfile=/usr/local/share/rabbitmq/certs/RMQ_SIEM_Client.pem&verify=verify_peer&server_name_indication=disable&auth_mechanism=external", "src-exchange": "pt.mpx.monitoring", "src-exchange-key": "monitoring"}'
   ```

Вы можете проверить корректность настройки на вкладке **Admin** → **Shovel Management** в веб-интерфейсе RabbitMQ на сервере MP 10 Core. В таблице **Shovel Management** в столбце **State** все экземпляры Shovel должны иметь статус **running**.

## Настройка очереди RabbitMQ

Инструкцию необходимо выполнить шесть раз — для каждого ключа маршрутизации:

- `agent.<Идентификатор коллектора>.query_config`;
- `agent.<Идентификатор коллектора>.rmq.heartbeat`;
- `agent.<Идентификатор коллектора>.command_package`;
- `agent.<Идентификатор коллектора>.event_package_ack`;
- `agent.<Идентификатор коллектора>.reset`;
- `agent.<Идентификатор коллектора>.job.*.query_artifacts`.

**Задача.** Чтобы настроить очередь:

1. В таблице выберите созданную ранее очередь с названием `fwd_agentlinux_<COLLECTOR_NAME>.v2`.
   → Откроется страница с информацией об очереди.
2. В блоке параметров **Bindings** → **Add binding to this queue** в поле **From exchange** введите `pt.mpx.agent.v2`.
3. В поле **Routing key** введите ключ маршрутизации (например, `agent.e829c7ce-2e57-42be-bc43-6eb442c0992f.query_config`).
   > [!note] Примечание
   > Идентификатор коллектора вы можете найти в его конфигурационном файле `/opt/core-agent/config.json`.
4. Нажмите **Bind**.

## Проверка подключения MP 10 Core к MP 10 Collector

**Задача.** Чтобы проверить подключение:

1. Войдите в веб-интерфейс MaxPatrol VM.
2. На странице **Система** → **Управление системой** выберите **Коллекторы**.
3. Если коллектор имеет статус **Недоступен**, перезапустите службу Core Agent.
   > [!note] Примечание
   > Если после перезапуска службы проблема сохраняется, необходимо сохранить файлы журналов коллектора и отправить их в службу технической поддержки.
4. Если коллектор имеет статус **Доступен**, создайте и запустите задачу на сбор данных с профилем HostDiscovery для проверки подключения к коллектору.
