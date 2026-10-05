---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "7806058635"
reuse_id: "7808899595"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7806058635"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Диагностика и решение проблем / Установка модуля Salt Minion завершается с ошибкой"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Установка модуля Salt Minion завершается с ошибкой

> [!info] Раздел: System.Collections.Hashtable[@{Id=7806058635; ReuseId=7808899595; Title=Установка модуля Salt Minion завершается с ошибкой; Depth=3; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Установка модуля Salt Minion завершается с ошибкой; Segments=System.Object[]; Index=111}.Id])
> @{Id=7806058635; ReuseId=7808899595; Title=Установка модуля Salt Minion завершается с ошибкой; Depth=3; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Установка модуля Salt Minion завершается с ошибкой; Segments=System.Object[]; Index=111}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7806058635)

---

## Решение

**Задача.** Чтобы решить проблему:

1. На сервере с установленной ролью Deployer перейдите в каталог `/opt/deployer/bin/`:
   ```bash
   cd /opt/deployer/bin/
   ```
2. Запустите утилиту Get-MinionDistrib.ps1:
   ```
   /opt/deployer/bin/Get-MinionDistrib.ps1
   ```
3. Укажите IP-адрес или FQDN сервера, на который необходимо установить модуль Salt Minion.
4. Выберите одну или несколько операционных систем, на которые необходимо установить модуль, и нажмите **Select**.
   → По завершении работы утилиты в каталоге появится архив `minion_dist_<IP-адрес или FQDN сервера, на который необходимо установить модуль Salt Minion>_<Номер версии роли Deployer>.gz`.
5. Скопируйте архив на сервер, на котором необходимо установить модуль Salt Minion.
6. Распакуйте архив:
   ```
   tar -xf minion_dist_<IP-адрес или FQDN сервера, на который необходимо установить модуль Salt Minion>_<Номер версии роли Deployer>.gz
   ```
7. Перейдите в каталог `minion_dist_<IP-адрес или FQDN сервера, на который необходимо установить модуль Salt Minion>_<Номер версии роли Deployer>` и запустите сценарий:
   ```
   ./install_minion.sh
   ```
8. На сервере с установленной ролью Deployer выполните команду:
   ```
   salt-key -A
   ```
9. В строке `Proceed? [n/Y]` введите `y` и нажмите **Enter**.
