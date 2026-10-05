---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Справочник-разработчика"
doc_id: "7023419019"
reuse_id: "7027143563"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7023419019"
section: "Справочник разработчика"
breadcrumb: "Справочник разработчика / Создание файла расширения аудита / Секция $loaders"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Секция $loaders

> [!info] Раздел: System.Collections.Hashtable[@{Id=7023419019; ReuseId=7027143563; Title=Секция $loaders; Depth=3; Path=Справочник разработчика / Создание файла расширения аудита / Секция $loaders; Segments=System.Object[]; Index=636}.Id])
> @{Id=7023419019; ReuseId=7027143563; Title=Секция $loaders; Depth=3; Path=Справочник разработчика / Создание файла расширения аудита / Секция $loaders; Segments=System.Object[]; Index=636}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7023419019)

---

Секция `$loaders` определяет порядок размещения данных, полученных из секций `$transformers` и `$extractors`.

Экстракторы возвращают список с различным количеством элементов, зависящим от типа экстрактора. Каждый элемент списка представляет из себя структуру, которую декларирует экстрактор, или структуру, встроенную в движок процесса расширения аудита.

## Обязательные поля секции

В секции `$loaders` необходимо последовательно заполнить следующие обязательные поля:

- `<Имя лоадера>` — определяет название лоадера;
- `$template` — объявляет используемый плагин (для лоадера всегда `mapping`). Все последующие использования `$template` объявляются внутри других полей и нужны для указания плагина, с помощью которого будет заполнено это поле. Поле может использоваться для передачи данных из секции `$transformers`. Источником данных для одного плагина может быть другой плагин, указанный в поле `$source`;
- `$target` — объявляет целевую сущность, для которой собираются данные;
- `$kind` — определяет режим работы экстрактора и может принимать следующие значения:

- `detect` — создание новой сущности. Например, если необходимо создать новую сущность Core.Software, выполнится итерация по всем элементам экстрактора с созданием сущностей Core.Software по количеству элементов в экстракторе. Значение может использоваться с подтипом `detect-for`, в этом случае итерация выполняется по возвращаемому значению `$origin` со всеми фильтрами;
- `scan` — обновление существующей сущности. Значение может использоваться с подтипами `scan-one` и `scan-table`. Подтип `scan-one` запускается на единственном возвращаемом экстрактором значении `$origin` со всеми фильтрами: если экстратор не вернул ни одного элемента, поля, которые заполняет экстрактор, не будут заполнены. Подтип `scan-table` запускается с полем `$table`, значение которого содержит результат запуска `$origin` со всеми фильтрами: все элементы экстрактора собираются в одно поле. Подтип `scan-table` используется вместе с полем `AdditionalProperties` и сериализацией в форматы JSON, YAML, XML.

- `$parent` — указывает родительскую сущность для заполняемой;
- `$params` — указывает источник для подстановки параметра в раздел `$origin`.

Вы можете описать условие заполнения сущности с помощью необязательного блока `$guard`. Например, условие для наследников класса Core.Software, у которых в поле `Name` указано значение `MS Office VBA`, а в поле `Version` — `'2019'`, будет выглядеть следующим образом:

```
$target: Core.Software
$kind: scan
$parent: OperatingSystem.Windows.WindowsHost
$guard: !eq-str
    $target.Name: MS Office VBA
    $target.Version: '2019'
```

Если предусловие в блоке определится как ложное, режим `scan` не будет определен.

В блоке `$guard` вы можете использовать:

- операторы сравнения:

- `!eq-str` — проверяет равенство строк: если все пары равны, возвращает `True`;
- `!neq-str` — проверяет неравенство строк: если все пары не равны, возвращает `True`;
- `!str-startswith` и `!str-endswith` — проверяют, что значение поля начинается или заканчивается с определенной строки. Если условие выполнено, возвращают `True`;
- `!str-in-str` — проверяет, содержится ли одна строка внутри другой строки;

- `$and` — проверяет, выполняются ли все условия внутри него: если условия выполняются, возвращает `True`;
- `$or` — проверяет, выполняется ли хотя бы одно условие внутри него: если условие выполняется, возвращает `True`;
- `$not` — инвертирует результаты проверки: возвращает `True`, если условие не выполняется, и `False`, если условие выполняется.

Например, если первое условие — наличие в полях `$target.Name` и `$target.Version` значений `MS Office VBA` и `'2019'` соответственно, а второе условие — наличие в поле `$target.Architecture` значения, отличного от `x86`, то блок будет выглядеть следующим образом:

```
$guard:
   $or:
   - $and:
       - !eq-str
         $target.Name: MS Office VBA
       - !eq-str
         $target.Version: '2019'
   - $not: !eq-str
       $target.Architecture: x86
```

## Секция $loaders в YAML-файле, содержащем секции $extractors, $transformers и $loaders

Структура секции `$loaders`:

```
$loaders:
    <Имя лоадера>:
    -   $template: <Имя плагина>
        $target: <Сущность, в которую запишутся данные>
        $kind: <Режим работы экстрактора>
        $parent: <Родительский узел для заполняемой сущности>
        $origin: <Источник, который содержит данные для передачи их в поля скана или в функцию обработки>
            $template: get_extractor_path
            $name: <Имя экстрактора>
            $params: <Имя параметра, объявленного в экстракторе>: <Данные, передаваемые в параметр>
        $field_map: <Поля модели актива для заполняемой сущности и специальное поле AdditionalProperties>
            <Поле>: <Значение>
            AdditionalProperties: <Поле для добавления в скан полей, отсутствующих в модели актива>
                <Поле>:
                    $serialize: <Источник данных>
                        $template: <Имя плагина>
                    $to: <Формат файла>
```

В секции `$loaders` могут быть использованы следующие плагины:

- `mapping` — определяет, что будет генерироваться XML-файл с информацией об активе;
- `apply_transformer` — позволяет применить функцию, созданную в секции `$transformers`, указать входные данные и записать результат в поле модели;
- `apply_resolver` — позволяет применить резолвер, созданный в секции `$transformers`, указать входные данные с помощью поля `$value` и записать результат в поле модели;
- `first_not_empty_string` — позволяет получить первое значение из списка (пустые значения не учитываются);
- `get_extractor_path` — объявляет источник данных для раздела `$origin` с помощью экстрактора из секции `$extractors`.

Пример 1:

```
$loaders:
    VirtualBox:
    -   $template: mapping
        $target: Core.Software
        $kind: detect
        $parent: OperatingSystem.UNIX.Linux.LinuxHost
        $origin:
            $template: get_extractor_path
            $name: VirtualBoxUnix
        $field_map:
            Name: VirtualBox
            Vendor: Oracle
            OsFamily: $parent.OsFamily
            Version: 6.1.5
            AdditionalProperties:
                SomeField1:
                    $serialize:
                        $template: apply_transformer
                        $name: get_virtualbox_version
                        $source: $row.Data
                    $to: json
                SomeField2: $parent.OsName
                SomeField3:
                    $serialize: $parent.IpAddress
                    $to: yaml
                SomeField4:
                    $template: apply_transformer
                    $name: get_virtualbox_version
                    $source: $row.Data
```

Пример 2:

```
$loaders:
    VirtualBox:
    -   $template: mapping
        $target: Software.Oracle.VirtualBox
        $kind: detect
        $parent: OperatingSystem.UNIX.Linux.LinuxHost
        $origin:
            $template: get_extractor_path
            $name: VirtualBoxUnix
        $field_map:
            AdditionalProperties:
                SomeUserField1: $row.StringData
                SomeUserField2:
                    $serialize: $row.UserSource
                    $to: json
                SomeUserField3:
                    $serialize:
                        $template: apply_transformer
                        $name: get_virtualbox_version
                        $source: $row.Data
                    $to: json
```
