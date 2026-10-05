---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Развертывание-MaxPatrol-VM"
doc_id: "4866226827"
reuse_id: "6192373771"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/4866226827"
section: "Развертывание MaxPatrol VM"
breadcrumb: "Развертывание MaxPatrol VM / Настройка обновления экспертных данных / Ручной перенос обновлений MaxPatrol VM в закрытый сегмент сети"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Ручной перенос обновлений MaxPatrol VM в закрытый сегмент сети

> [!info] Раздел: System.Collections.Hashtable[@{Id=4866226827; ReuseId=6192373771; Title=Ручной перенос обновлений MaxPatrol VM в закрытый сегмент сети; Depth=3; Path=Развертывание MaxPatrol VM / Настройка обновления экспертных данных / Ручной перенос обновлений MaxPatrol VM в закрытый сегмент сети; Segments=System.Object[]; Index=36}.Id])
> @{Id=4866226827; ReuseId=6192373771; Title=Ручной перенос обновлений MaxPatrol VM в закрытый сегмент сети; Depth=3; Path=Развертывание MaxPatrol VM / Настройка обновления экспертных данных / Ручной перенос обновлений MaxPatrol VM в закрытый сегмент сети; Segments=System.Object[]; Index=36}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/4866226827)

---

Если между локальными серверами обновлений отсутствует сетевое взаимодействие, вам нужно вручную перенести обновления в закрытый сегмент сети для последующего обновления MaxPatrol VM.

**Задача.** Чтобы вручную перенести обновления в закрытый сегмент сети:

1. На локальном сервере обновлений в демилитаризованной зоне запустите получение обновлений с глобального сервера обновлений Positive Technologies:
   ```bash
   sudo /opt/pt/pt-update-mirror/bin/pt-update-mirror repository update
   ```
2. Запустите экспорт репозитория с обновлениями в файл:
   ```bash
   sudo /opt/pt/pt-update-mirror/bin/pt-update-mirror repository export --repo vm-expertise <Название файла>.tgz
   ```
   > [!note] Примечание
   > Выполнение команды экспорта без ключа `--repo <Название репозитория>` позволяет экспортировать все репозитории базы обновлений в указанный архив. Вы можете просмотреть список репозиториев в базе обновлений локального сервера с помощью команды `sudo /opt/pt/pt-update-mirror/bin/pt-update-mirror repository view`.
3. Скопируйте с помощью внешнего носителя полученный файл архива в каталог, принадлежащий пользователю pt-update-mirror, на локальном сервере обновлений в закрытом сегменте сети.
4. На локальном сервере обновлений в закрытом сегменте сети импортируйте обновления из скопированного файла архива:
   ```bash
   sudo /opt/pt/pt-update-mirror/bin/pt-update-mirror repository import <Путь к архиву>/<Название архива>.tgz
   ```

> Обновления MaxPatrol VM перенесены.
