---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Развертывание-MaxPatrol-VM"
doc_id: "6079636363"
reuse_id: "6469712139"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6079636363"
section: "Развертывание MaxPatrol VM"
breadcrumb: "Развертывание MaxPatrol VM / Автоматическое развертывание MaxPatrol VM"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Автоматическое развертывание MaxPatrol VM

> [!info] Раздел: System.Collections.Hashtable[@{Id=6079636363; ReuseId=6469712139; Title=Автоматическое развертывание MaxPatrol VM; Depth=2; Path=Развертывание MaxPatrol VM / Автоматическое развертывание MaxPatrol VM; Segments=System.Object[]; Index=9}.Id])
> @{Id=6079636363; ReuseId=6469712139; Title=Автоматическое развертывание MaxPatrol VM; Depth=2; Path=Развертывание MaxPatrol VM / Автоматическое развертывание MaxPatrol VM; Segments=System.Object[]; Index=9}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/6079636363)

---

Для автоматического развертывания MaxPatrol VM на Linux вам понадобится сценарий установки `install-aio.sh` из комплекта поставки. Запускать сценарий в интерфейсе терминала необходимо с правами суперпользователя (root).

**Задача.** Чтобы развернуть MaxPatrol VM:

1. В каталоге `deploy_manifests` откройте на редактирование файл манифеста `VM-AIO.yaml` и в блок параметров `Instances` → `ManagementAndConfiguration` добавьте параметр `ExpertDataUpdateMethod` со значением `Online`, а также параметр `PackagesSourceUri`, в качестве значения которого укажите адрес сервера для получения экспертного контента:
   ```
   ManagementAndConfiguration:
   type: ManagementAndConfiguration
   host: local
   order: 3
   params:
   ExpertDataUpdateMethod: Online
   PackagesSourceUri: <Адрес сервера для получения экспертного контента>
   ```
   > [!warning] Внимание
   > Параметры и их значения в файле манифеста должны быть указаны с соблюдением синтаксиса языка YAML.
   > [!note] Примечание
   > Если вы устанавливаете MaxPatrol VM в сегменте сети с прямым подключением к интернету, в качестве значения параметра `PackagesSourceUri` необходимо указать один из адресов глобального сервера обновлений — `https://update.ptsecurity.ru/packman/v1/` или `https://update.ptsecurity.com/packman/v1/`; если в изолированном сегменте сети без прямого подключения к интернету — адрес локального сервера обновлений в формате `http://<Адрес сервера>:<Порт>/packman/v1/`. Указанный в параметре порт локального сервера обновлений должен быть открыт для входящих соединений. Для подключения по протоколам HTTP и HTTPS по умолчанию используются порты 8553 и 8743 соответственно.
2. Добавьте:
   → В блок параметров `Instances` → `SqlStorage` параметр `PgHardDiskType` со значением `SSD`.
   ```
   SqlStorage:
   type: SqlStorage
   host: local
   params:
   PgHardDiskType: SSD
   order: 1
   ```
   → В блок параметров `Instances` → `Core` параметр `AssetGridVersionRangeModeEnabled` со значением `True`.
   ```
   Core:
   type: Core
   host: local
   order: 6
   params:
   AssetGridVersionRangeModeEnabled: True
   ```
3. Сохраните изменения и закройте файл.
4. Запустите сценарий установки `install-aio.sh` от имени суперпользователя (root).
5. Выберите манифест **VM-AIO** и нажмите **OK**.
   → Начнется проверка сервера на соответствие программным и аппаратным требованиям.
   → Если сервер не соответствует требованиям, отобразится сообщение `Error`. Описание параметров и ошибок приведено в разделе «[[О проверке серверов перед установкой ролей|Параметры соответствия сервера программным и аппаратным требованиям]]» Руководства администратора.
   > [!note] Примечание
   > При авторазвертывании сообщения `Warning` автоматически игнорируются.
6. Ознакомьтесь с условиями лицензионного соглашения и нажмите **I Accept**, чтобы принять их.
   → Начнется установка пакетов. По завершении установки появится сообщение `Application deploy manifest successfully applied`.

В результате установки роли (в том числе и неуспешной), в каталоге, из которого был запущен сценарий `install-aio.sh`, формируется каталог `installReports` с отчетами об установке. Отчеты сохраняются в HTML-файлах и содержат информацию о системе и журналы установки. Вы можете передавать отчеты в рамках запроса в техническую поддержку для анализа проблем.

После завершения установки нужно:

1. [[Активация PT MC [Развертывание MaxPatrol VM]|активировать]] PT MC;
2. активировать лицензию, приобретенную вашей организацией;
3. выполнить харденинг MaxPatrol VM.
