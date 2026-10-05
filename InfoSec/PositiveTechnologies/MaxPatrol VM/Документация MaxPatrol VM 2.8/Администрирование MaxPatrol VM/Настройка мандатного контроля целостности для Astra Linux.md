---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "7555526155"
reuse_id: "9284587147"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7555526155"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Диагностика и решение проблем / Справочная информация / Настройка мандатного контроля целостности для Astra Linux"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Настройка мандатного контроля целостности для Astra Linux

> [!info] Раздел: System.Collections.Hashtable[@{Id=7555526155; ReuseId=9284587147; Title=Настройка мандатного контроля целостности для Astra Linux; Depth=4; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Справочная информация / Настройка мандатного контроля целостности для Astra Linux; Segments=System.Object[]; Index=124}.Id])
> @{Id=7555526155; ReuseId=9284587147; Title=Настройка мандатного контроля целостности для Astra Linux; Depth=4; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Справочная информация / Настройка мандатного контроля целостности для Astra Linux; Segments=System.Object[]; Index=124}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7555526155)

---

Если MaxPatrol VM устанавливается на Astra Linux и включен мандатный контроль целостности (МКЦ), для учетной записи, которая используется при установке компонентов системы, необходим максимальный уровень целостности (63). Вы можете проверить, включен ли МКЦ, с помощью команды `astra-mic-control status`. Также вы можете просмотреть уровень МКЦ пользователя с помощью команды `sudo pdpl-user <Имя пользователя>`.

**Задача.** Чтобы изменить уровень целостности,

1. выполните команды:
   ```bash
   sudo gpasswd -a <Имя пользователя> astra-admin
   sudo pdpl-user <Имя пользователя> -i <Уровень целостности>
   ```

После установки MaxPatrol VM рекомендуется установить прежнее значение уровня целостности.
