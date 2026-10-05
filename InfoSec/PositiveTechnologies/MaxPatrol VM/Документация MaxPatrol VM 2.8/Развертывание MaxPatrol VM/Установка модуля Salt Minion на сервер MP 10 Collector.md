---
tags:
  - "maxpatrol-vm"
  - "pt-documentation"
  - "vm-2.8"
  - "pt/Развертывание-MaxPatrol-VM"
doc_id: "3230829195"
reuse_id: "6192277515"
source: "https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/3230829195"
section: "Развертывание MaxPatrol VM"
breadcrumb: "Развертывание MaxPatrol VM / Установка компонента MP 10 Collector / Установка модуля Salt Minion на сервер MP 10 Collector"
product: MaxPatrol VM 2.8
mirrored: 2026-10-05
---

# Установка модуля Salt Minion на сервер MP 10 Collector

> [!info] Раздел: System.Collections.Hashtable[@{Id=3230829195; ReuseId=6192277515; Title=Установка модуля Salt Minion на сервер MP 10 Collector; Depth=3; Path=Развертывание MaxPatrol VM / Установка компонента MP 10 Collector / Установка модуля Salt Minion на сервер MP 10 Collector; Segments=System.Object[]; Index=19}.Id])
> @{Id=3230829195; ReuseId=6192277515; Title=Установка модуля Salt Minion на сервер MP 10 Collector; Depth=3; Path=Развертывание MaxPatrol VM / Установка компонента MP 10 Collector / Установка модуля Salt Minion на сервер MP 10 Collector; Segments=System.Object[]; Index=19}.Path
> [Источник на help.ptsecurity.com](https://help.ptsecurity.com/ru-RU/projects/vm/2.8/help/3230829195)

---

> [!warning] Внимание
> Если на сервере, на котором развертывается компонент, уже установлена любая из ролей, установка модуля Salt Minion не требуется.

> [!warning] Внимание
> На сервере, на который устанавливается модуль Salt Minion, должен быть открыт доступ по SSH (например, через порт 22/TCP).

**Задача.** Чтобы установить модуль Salt Minion:

1. Если на сервере MP 10 Collector есть файл `/etc/salt/pki/minion/minion_master.pub`, удалите его:
   ```
   rm /etc/salt/pki/minion/minion_master.pub
   ```
2. Если MP 10 Collector устанавливается на Astra Linux, на сервере MP 10 Collector отключите обязательный ввод пароля для выполнения команды `sudo`:
   ```bash
   sudo astra-sudo-control disable
   ```
3. Если MP 10 Collector устанавливается на Debian, на сервере компонента установите утилиту `sudo`, выполнив в интерфейсе терминала команду от имени суперпользователя (root):
   ```bash
   apt-get install sudo
   ```
4. Если MP 10 Collector устанавливается на Debian, на сервере компонента отключите обязательный ввод пароля для выполнения команды `sudo`, добавив в файл `etc/sudoers` строку:
   ```xml
   <Логин учетной записи, от имени которой устанавливается компонент> ALL=(ALL:ALL) NOPASSWD: ALL
   ```
5. На сервере с установленной ролью Deployer запустите сценарий:
   ```
   /var/lib/deployer/role_packages/Deployer_<Номер версии>/deploy_minion.sh
   ```
6. В открывшемся окне введите IP-адрес или FQDN сервера MP 10 Collector и нажмите кнопку **OK**.
   > [!warning] Внимание
   > Если в параметре `HostAddress` указать значение `localhost`, установка завершится с ошибкой. Для всех конфигураций систем необходимо указать IP-адрес или FQDN сервера.
7. В открывшемся окне введите логин учетной записи, от имени которой устанавливается компонент, на сервере MP 10 Collector и нажмите кнопку **OK**.
8. В окне **Info** нажмите **OK**.
9. Введите пароль учетной записи с правами суперпользователя (root) на сервере MP 10 Collector.
   → Запустится установка модуля Salt Minion.
10. Если требуется, введите FQDN сервера, на который устанавливается модуль, и нажмите **OK**.
    → По завершении установки появится сообщение `Minion on '<IP-адрес или FQDN сервера>' successfully installed`.
