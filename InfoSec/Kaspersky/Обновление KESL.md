
Проверить задачу обновления:

```shell
sudo /opt/kaspersky/kesl/bin/kesl-control --get-task-list
```

Проверить, что присутствует задача №6

Выгружаем файл настроек задачи обновления для задачи:

```shell
sudo /opt/kaspersky/kesl/bin/kesl-control --get-settings ID --file ./update.xml
```

Для задачи №6:

```shell
sudo /opt/kaspersky/kesl/bin/kesl-control --get-settings 6 --file ./update.xml
```

Открываем файл настроек:

```shell
sudo nano update.xml
```

```plain
SourceType=KLServers
UseKLServerWhenUnavailable=Yes
ConnectionTimeout=10
ApplicationUpdateMode=DownloadOnly
```

меняем файл настроек задачи:

```plain
SourceType=Custom
UseKLServerWhenUnavailable=Yes
ConnectionTimeout=10
ApplicationUpdateMode=Disabled
[CustomSources.item_0000]
URL=/var/update
Enabled=Yes
```

импортируем настройки задачи

```shell
sudo /opt/kaspersky/kesl/bin/kesl-control --set-settings 6 --file ./update.xml
```

запускаем задачу

```shell
sudo /opt/kaspersky/kesl/bin/kesl-control --start-task 6
```

проверяем статус обновления

```shell
sudo /opt/kaspersky/kesl/bin/kesl-control --app-info
```


