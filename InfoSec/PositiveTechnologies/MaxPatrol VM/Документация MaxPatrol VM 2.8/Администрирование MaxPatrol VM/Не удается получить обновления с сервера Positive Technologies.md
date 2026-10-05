---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Администрирование-MaxPatrol-VM"
doc_id: "5906897675"
reuse_id: "5913640203"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5906897675"
section: "Администрирование MaxPatrol VM"
breadcrumb: "Администрирование MaxPatrol VM / Диагностика и решение проблем / Не удается получить обновления с сервера Positive Technologies"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Не удается получить обновления с сервера Positive Technologies

> [!info] Раздел: System.Collections.Hashtable[@{Id=5906897675; ReuseId=5913640203; Title=Не удается получить обновления с сервера Positive Technologies; Depth=3; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Не удается получить обновления с сервера Positive Technologies; Segments=System.Object[]; Index=106}.Id])
> @{Id=5906897675; ReuseId=5913640203; Title=Не удается получить обновления с сервера Positive Technologies; Depth=3; Path=Администрирование MaxPatrol VM / Диагностика и решение проблем / Не удается получить обновления с сервера Positive Technologies; Segments=System.Object[]; Index=106}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/5906897675)

---

## Проблема

Не удается получить пакеты с обновлениями экспертных данных с сервера Positive Technologies.

## Возможные причины

Возможными причинами проблемы являются ошибки в работе компонента PT MC или локального сервера обновлений, неправильная настройка этих компонентов, а также недоступность сервера Positive Technologies.

## Решение

**Задача.** Чтобы добавить пакет обновлений в систему:

1. Извлеките файлы пакетов обновлений из архива, полученного от службы технической поддержки.
   > [!note] Примечание
   > Файлы пакетов обновлений имеют расширение `.pkg`.
2. Скопируйте файлы пакетов обновлений в каталог `/var/lib/deployed-roles/mc-application/managementandconfiguration/data/resources/local-packages/` на сервере с ролью Management and Configuration.
   → Система автоматически установит пакеты обновлений из каталога `local-packages`.

Вы можете найти информацию об установленных пакетах обновлений на странице **Система** → **Управление системой** → **База уязвимостей**.
