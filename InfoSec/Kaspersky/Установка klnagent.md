
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



