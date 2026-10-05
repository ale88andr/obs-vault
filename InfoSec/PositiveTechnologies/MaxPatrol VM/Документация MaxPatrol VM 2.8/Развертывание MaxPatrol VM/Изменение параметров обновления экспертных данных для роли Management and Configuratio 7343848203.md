---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Развертывание-MaxPatrol-VM"
doc_id: "7343848203"
reuse_id: "7347023115"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7343848203"
section: "Развертывание MaxPatrol VM"
breadcrumb: "Развертывание MaxPatrol VM / Настройка обновления экспертных данных / Изменение параметров обновления экспертных данных для роли Management and Configuration"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Изменение параметров обновления экспертных данных для роли Management and Configuration

> [!info] Раздел: System.Collections.Hashtable[@{Id=7343848203; ReuseId=7347023115; Title=Изменение параметров обновления экспертных данных для роли Management and Configuration; Depth=3; Path=Развертывание MaxPatrol VM / Настройка обновления экспертных данных / Изменение параметров обновления экспертных данных для роли Management and Configuration; Segments=System.Object[]; Index=29}.Id])
> @{Id=7343848203; ReuseId=7347023115; Title=Изменение параметров обновления экспертных данных для роли Management and Configuration; Depth=3; Path=Развертывание MaxPatrol VM / Настройка обновления экспертных данных / Изменение параметров обновления экспертных данных для роли Management and Configuration; Segments=System.Object[]; Index=29}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/7343848203)

---

**Задача.** Чтобы изменить параметры роли:

1. На сервере с установленной ролью Deployer распакуйте архив `pt_managementandconfiguration_<Номер версии>.tar.gz` из комплекта поставки:
   ```
   tar -xf pt_managementandconfiguration_<Номер версии>.tar.gz
   ```
2. Запустите сценарий:
   ```
   pt_managementandconfiguration_<Номер версии>/install.sh
   ```
3. В открывшемся окне нажмите кнопку **Yes**.
4. Выберите вариант с идентификатором приложения роли.
5. Выберите вариант с идентификатором экземпляра роли.
   → Откроется окно для выбора набора параметров.
6. Выберите вариант **Advanced configuration**.
   → Откроется страница со списком параметров.
7. В качестве значения параметра `ExpertDataUpdateMethod` выберите:
   - Если обновление экспертных данных будет осуществляться напрямую с сервера обновлений Positive Technologies или с помощью локального сервера обновлений — `Online`.
   - Если вручную — `Offline`.
8. В качестве значения параметра `PackagesSourceUri` укажите:
   - Если MaxPatrol VM установлен в сегменте сети с прямым подключением к интернету — один из адресов глобального сервера обновлений — `https://update.ptsecurity.ru/packman/v1/` или `https://update.ptsecurity.com/packman/v1/`.
   - Если в изолированном сегменте сети без прямого подключения к интернету — адрес локального сервера обновлений в формате `http://<Адрес сервера>:<Порт>/packman/v1/`.
   > [!note] Примечание
   > Указанный в параметре порт локального сервера обновлений должен быть открыт для входящих соединений. Для подключения по протоколам HTTP и HTTPS по умолчанию используются порты 8553 и 8743 соответственно.
9. Нажмите кнопку **ОК**.
   → Начнется изменение конфигурации роли. По его завершении появится сообщение `Deployment configuration successfully applied`.
10. Нажмите кнопку **ОК**.

> Параметры роли изменены.
