### Вывод лога в текстовый файл

Если необходимо вывести лог в текстовый формат, используйте конструкцию: **[команда] > [путь_к_текстовому_файлу]**

```shell
journalctl -b --no-pager > ~/journalctl.log
```

### Работа с логами

Лог с момента запуска системы:

```shell
journalctl -b
```

Лог с момента запуска системы с расшифровкой ошибок:

```shell
journalctl -xb
```

Последние записи журнала, и продолжать выводить новые, при добавлении их в журнал:

```shell
journalctl -f
```

Просмотр информации о недавних событиях:

```shell
journalctl -n
```

По умолчанию в терминал выводится информация о последних 10 событиях. С опцией -n можно указать необходимое число событий:

```shell
journalctl -n 20
```

Для фильтрации по дате и времени важны два ключа

`--since` — вывод от такого-то момента времени
`--until` — вывод до такого-то момента времени

Примеры:

Формат: YYYY-MM-DD HH:MM:SS

```shell
journalctl --since "20122-05-03 00:01" --until "2022-05-04 00:02"
```

Слова: «yesterday», «today», «tomorrow», «now»:

```shell
journalctl --since "yesterday" --until "2022-05-04 00:02"
```

Показывать в реальном времени все записи, независимо от их размера и кодировки:

```shell
journalctl -af
```

Сообщения ядра:

```shell
journalctl -k
```

Сообщения конкретного процесса:

```shell
journalctl _PID=1
```

Сообщения конкретного приложения или службы:

```shell
journalctl -u nginx
```

Сообщения процессов, запущенных от имени пользователя:

```shell
journalctl _UID=1001
```

### Ограничение размера журнала

Если `journald` настроен, чтобы сохранять журналы после перезагрузки, то по умолчанию размер журнала ограничен 10% от объема файлового раздела и максимально может занять 4Гб дискового пространства.

Максимальный объем журнала можно скорректировать, раскомментировав и отредактировав следующий параметр в файле конфигурации `/etc/systemd/journald.conf`.

```shell
sudo nano /etc/systemd/journald.conf  
```

В блоке `[journal]` надо изменить параметр `systemmaxuse`, например `systemmaxuse = 20M`.

### Удаление журналов и определение текущего использования дискового пространства

Удалить файлы архивных журналов, можно вручную с помощью rm или использовать journalctl.

Удалить журналы, оставив только последние 50M:

```shell
journalctl --vacuum-size=50M
```

Удалить журналы, оставив журналы только за последние 5 дней:

```shell
journalctl --vacuum-time=5d
```

Определить, сколько места занимает журнал на диске, используя флаг `—disk-usage`:

```shell
journalctl --disk-usage
```

### Фильтрация по приоритету

Фильтрация может применяться к приоритету сообщения, если вы хотите отфильтровать конкретное сообщение, такое как “warning” или “error” и т.д.

Перечислены все уровни приоритета:

|Приоритет|Код|
|---|---|
|0|emerg|
|1|alert|
|2|crit|
|3|err|
|4|warning|
|5|notice|
|6|info|
|7|debug|

Пример:

```shell
journalctl -p 3 -b
```

Или

```shell
journalctl -p err -b
```

### Справка по утилите

```shell
journalctl --help
journalctl -h
```
