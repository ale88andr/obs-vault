---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Развертывание-MaxPatrol-VM"
doc_id: "11189457803"
reuse_id: "11189789707"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/11189457803"
section: "Развертывание MaxPatrol VM"
breadcrumb: "Развертывание MaxPatrol VM / Настройка подключения к Collector, расположенному в недоверенном сегменте / Настройка подключения MP 10 Core к Collector на Windows"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Настройка подключения MP 10 Core к Collector на Windows

> [!info] Раздел: System.Collections.Hashtable[@{Id=11189457803; ReuseId=11189789707; Title=Настройка подключения MP 10 Core к Collector на Windows; Depth=3; Path=Развертывание MaxPatrol VM / Настройка подключения к Collector, расположенному в недоверенном сегменте / Настройка подключения MP 10 Core к Collector на Windows; Segments=System.Object[]; Index=24}.Id])
> @{Id=11189457803; ReuseId=11189789707; Title=Настройка подключения MP 10 Core к Collector на Windows; Depth=3; Path=Развертывание MaxPatrol VM / Настройка подключения к Collector, расположенному в недоверенном сегменте / Настройка подключения MP 10 Core к Collector на Windows; Segments=System.Object[]; Index=24}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/11189457803)

---

После установки ролей Collector и RMQ Message Bus необходимо настроить подключение MP 10 Core к Collector. Для этого необходимо добавить сертификаты для RabbitMQ и настроить плагин Shovel для отправки сообщений из очереди RabbitMQ.

## Добавление сертификатов для RabbitMQ

**Задача.** Чтобы добавить сертификаты:

1. На сервере с ролью Deployer создайте SLS-файл `/var/lib/deployer/role_packages/deployer/generate_custom_cert.sls`.
   → Пример содержимого файла:
   ```
   {%- macro generate_remote_signed_cert(common_name, cert, key, dns_subjects=['example.ru', 'localhost'], ip_subjects=['127.0.0.1', '10.10.248.163']) %}
   {{ key }}:
   x509.private_key_managed:
   - bits: 2048
   - backup: True
   {% set dns_subjects = (dns_subjects + [common_name, 'localhost'])|unique %}
   {% set ip_subjects = (ip_subjects + ['127.0.0.1'])|unique %}
   {% set subjects = [] %}
   {% for name in dns_subjects %}{% do subjects.append('DNS:'~name) %}{%endfor%}
   {% for ip in ip_subjects %}{% do subjects.append('IP:'~ip) %}{%endfor%}
   Certificate {{ cert }}:
   x509.certificate_managed:
   - name: {{ cert }}
   - ca_server: {{pillar.deployer.pki.ca_server}}
   - signing_policy: {{pillar.deployer.pki.ssl_signing_policy}}
   - public_key: {{ key }}
   - private_key: {{ key }}
   - CN: {{ common_name }}
   - subjectAltName: {{subjects|join(', ')}}
   - keyUsage: "digitalSignature, dataEncipherment, keyEncipherment, keyAgreement"
   - extendedkeyUsage: "serverAuth, clientAuth"
   - C: RU
   - ST: None
   - L: Moscow
   - O: Company Name
   - OU: None
   - Mail: companyname@example.com
   - days_valid: 1825
   - days_remaining: 10
   - require:
   - x509: {{ key }}
   - backup: True
   {%- endmacro %}
   {{ generate_remote_signed_cert('Test', '/tmp/RMQ_Server.crt', '/tmp/RMQ_Server.pem', dns_subjects= ['localhost', 'test.ru', 'example.ru'], ip_subjects=['127.0.0.1', '10.10.248.163']) }}
   ```
   > [!note] Примечание
   > В качестве значений параметров `dns_subjects` и `ip_subjects` необходимо указать IP-адреса и FDQN серверов, на которых установлены коллекторы.
2. Для создания ключа и сертификата выполните команду:
   ```bash
   sudo salt-call state.apply deployer.generate_custom_cert
   ```
   → Будут созданы ключ `RMQ_Server.pem` и сертификат `RMQ_Server.crt` внутри временного каталога `/tmp`.
3. Переместите файлы сертификатов с сервера с ролью Deployer на сервер с ролью Collector в соответствии с таблицей ниже.
4. На сервере с ролью Deployer выполните команды для генерации сертификатов `RMQ_Agent_Client.csr`, `RMQ_Agent_Client.crt`:
   ```bash
   openssl req -new -sha256 -nodes -newkey rsa:2048 -keyout RMQ_Agent_Client.key -subj '/CN=agent' -out RMQ_Agent_Client.csr
   openssl x509 -req -in RMQ_Agent_Client.csr -CA /opt/deployer/pki/rootCA.crt -CAkey /opt/deployer/pki/rootCA.key -days 356 -CAcreateserial -out RMQ_Agent_Client.crt
   ```
5. Замените существующие сертификаты `rootCA.crt`, `RMQ_Agent_Client.csr`, `RMQ_Agent_Client.crt` на сервере с ролью Collector (например, в папке `C:\Program Files (x86)\Positive Technologies\MP 10 Collector\.install\scripts\Certificates`) сертификатами, сгенерированными на сервере с ролью Deployer.
6. На сервере с ролью Collector перезапустите RabbitMQ с помощью команды:
   ```
   rabbitmqcfg restart
   ```

**Соответствие файлов сертификатов и путей к файлам**

| Имя файла на сервере с ролью Deployer | Путь к файлу на сервере с ролью Collector |
| --- | --- |
| `/tmp/RMQ_Server.crt` | `C:\ProgramData\RabbitMQ\tls\RMQ_Server.crt` |
| `/tmp/RMQ_Server.pem` | `C:\ProgramData\RabbitMQ\tls\RMQ_Server.pem` |
| `/var/lib/deployer/role_packages/deployer/rootCA.crt` | `C:\ProgramData\RabbitMQ\tls\rootCA.crt` |

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
2. В блоке параметров **Bindings** → **Add binding to this queue** в поле **From exchange** введите `pt.mpx.agent.v2`.
3. В поле **Routing key** введите ключ маршрутизации (например, `agent.e829c7ce-2e57-42be-bc43-6eb442c0992f.query_config`).
   > [!note] Примечание
   > Идентификатор коллектора вы можете найти в его конфигурационном файле по пути `C:\ProgramData\Positive Technologies\MaxPatrol 10 Agent\config\agent.conf`.
4. Нажмите **Bind**.

## Проверка подключения MP 10 Core к MP 10 Collector

**Задача.** Чтобы проверить подключение:

1. Войдите в веб-интерфейс MaxPatrol VM.
2. На странице **Система** → **Управление системой** выберите **Коллекторы**.
3. Если коллектор имеет статус **Недоступен**, перезапустите службу Core Agent.
   > [!note] Примечание
   > Если после перезапуска службы проблема сохраняется, необходимо сохранить файлы журналов коллектора и отправить их в службу технической поддержки.
4. Если коллектор имеет статус **Доступен**, создайте и запустите задачу на сбор данных с профилем HostDiscovery для проверки подключения к коллектору.
