
# Подготовка Astra Linux для удаленной установки

https://support.kaspersky.ru/ksc-linux/14.2/137593

```
sudo nano /home/opfradmin/.bashrc
```

```shell
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

```shell
sudo nano /etc/systemd/logind.conf
```

```shell
KillUserProcesses=no
KillExcludeUsers=opfradmin
```

```shell
sudo visudo
```

```shell
%astra-admin ALL=(ALL:ALL) NOPASSWD:ALL
```

**Проверка одного порта (TCP):**

Код

```shell
nc -zv <IP_адрес_удаленного_хоста> <номер_порта>
```

`z` (0) - сканирование (без отправки данных), `v` - подробный вывод. 

**Проверка диапазона портов:**

Код

```shell
nc -zv <IP_адрес_удаленного_хоста> <диапазон_портов>
```

- Например, для портов с 80 по 83: `nc -vz 8.8.8.8 80-83`.

