---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Обновление-MaxPatrol-VM"
doc_id: "102330507"
reuse_id: "4470300427"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/102330507"
section: "Обновление MaxPatrol VM"
breadcrumb: "Обновление MaxPatrol VM / Обновление с помощью дистрибутивов / Обновление компонента MP 10 Collector на Microsoft Windows"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Обновление компонента MP 10 Collector на Microsoft Windows

> [!info] Раздел: System.Collections.Hashtable[@{Id=102330507; ReuseId=4470300427; Title=Обновление компонента MP 10 Collector на Microsoft Windows; Depth=3; Path=Обновление MaxPatrol VM / Обновление с помощью дистрибутивов / Обновление компонента MP 10 Collector на Microsoft Windows; Segments=System.Object[]; Index=50}.Id])
> @{Id=102330507; ReuseId=4470300427; Title=Обновление компонента MP 10 Collector на Microsoft Windows; Depth=3; Path=Обновление MaxPatrol VM / Обновление с помощью дистрибутивов / Обновление компонента MP 10 Collector на Microsoft Windows; Segments=System.Object[]; Index=50}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/102330507)

---

> [!warning] Внимание
> В результате обновления компонентов установленные ранее пользовательские сертификаты безопасности будут заменены сертификатами из комплекта поставки. Если для работы компонентов использовались сертификаты безопасности, отличные от стандартных, необходимо снова настроить их после обновления.

Для обновления вам потребуется архив `pt_agent-windows_<Номер версии>.tar.gz` из комплекта поставки.

**Задача.** Чтобы обновить компонент MP 10 Collector:

1. На сервере с установленной ролью Collector запустите файл `MPXAgentSetup_<Номер версии>.exe`.
   → Откроется окно мастера обновления.
2. Ознакомьтесь по ссылке с текстом лицензионного соглашения.
3. Установите флажок **Я принимаю условия лицензионного соглашения** и нажмите **Обновить**.
4. По завершении обновления нажмите **Закрыть**.
5. В командной строке Microsoft Windows выполните команду от имени администратора:
   ```
   coreagentcfg set -p AgentRMQVirtualHost mpx
   ```
