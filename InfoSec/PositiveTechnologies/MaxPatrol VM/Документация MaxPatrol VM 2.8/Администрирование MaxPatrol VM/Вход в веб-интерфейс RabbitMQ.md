---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "1576622475"
reuse_id: "5410477835"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/1576622475"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Диагностика и решение проблем / Вход в веб-интерфейс RabbitMQ"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Вход в веб-интерфейс RabbitMQ

> [!info] Раздел: System.Collections.Hashtable[@{Id=1576622475; ReuseId=5410477835; Title=Вход в веб-интерфейс RabbitMQ; Depth=3; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Вход в веб-интерфейс RabbitMQ; Segments=System.Object[]; Index=100}.Id])
> @{Id=1576622475; ReuseId=5410477835; Title=Вход в веб-интерфейс RabbitMQ; Depth=3; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Вход в веб-интерфейс RabbitMQ; Segments=System.Object[]; Index=100}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/1576622475)

---

Перед входом в веб-интерфейс RabbitMQ необходимо убедиться, что правила межсетевого экрана разрешают входящее соединение от рабочей станции администратора к серверу RabbitMQ через порт 15672/TCP.

**Задача.** Чтобы войти в веб-интерфейс RabbitMQ:

1. В адресной строке браузера введите:
   ```
   http://<IP-адрес или FQDN сервера RabbitMQ>:15672
   ```
2. Введите логин и пароль.
   > [!note] Примечание
   > По умолчанию для входа в веб-интерфейс RabbitMQ используются логин `Administrator` и пароль `P@ssw0rd`.
3. Нажмите кнопку **Login**.
   → Отобразится страница **Overview**.

Если веб-интерфейс недоступен, необходимо проверить статус контейнера, активность процесса RabbitMQ внутри контейнера и прослушивание порта 15672/TCP.

При успешной аутентификации в правом верхнем углу в выпадающем списке **Virtual host** будут доступны для выбора следующие варианты: **All**, **\\**, **mpx**, **siem**.
