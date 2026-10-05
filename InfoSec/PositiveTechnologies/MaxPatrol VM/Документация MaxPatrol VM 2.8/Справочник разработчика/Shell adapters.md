---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "6997329547"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6997329547"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Создание файла расширения аудита / Секция $extractors / Shell adapters"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Shell adapters

> [!info] Раздел: System.Collections.Hashtable[@{Id=6997329547; ReuseId=; Title=Shell adapters; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $extractors / Shell adapters; Segments=System.Object[]; Index=629}.Id])
> @{Id=6997329547; ReuseId=; Title=Shell adapters; Depth=4; Path=Справочник разработчика / Создание файла расширения аудита / Секция $extractors / Shell adapters; Segments=System.Object[]; Index=629}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6997329547)

---

Экстратор shell adapters используется для реализации пользовательских схем с помощью плагинов.

Структура экстрактора:

```
$extractors:
    <Имя экстрактора>:
        $template: <Имя плагина>
        $command: <Команда>
        $params:
            <Имя параметра>: <Тип данных>
```

Все поля, кроме поля `$params`, являются обязательными. Поле `$params` позволяет динамически задавать параметры для транспортного запроса во время выполнения аудита.

Допустимые имена плагинов: `windows_shell`, `unix_shell`, `device_shell`, `powershell`. Для плагина `powershell` может использоваться необязательное поле `$scheduler` со следующими значениями:

- `True` — запуск команды через планировщик задач;
- `False` — прямой запуск через процесс Windows.

Пример с использованием плагина `windows_shell`:

```
$extractors:
    RawReverseDNSLookup:
        $template: windows_shell
        $command: ping -n 1 -a ?host_field
```

Пример с использованием плагина `unix_shell`:

```
$extractors:
    ExtName:
        $template: unix_shell
        $command: 'tail -n ?n_param /var/log/some.log'
        $params:
            n_param: int
```
