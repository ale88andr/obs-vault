## Настройка антивирусных исключений

В антивирусные исключения добавьте:

1. Директории:
    - `/run/`.
    - `/var/run/`.
    - `/etc/nova/`.
    - `/var/lib/nova/`.
    - `/etc/neutron/`.
    - `/var/lib/neutron/`.
    - `/etc/glance/`.
    - `/var/lib/glance/`.
    - `/etc/cinder/`.
    - `/var/lib/cinder/`.
    - `/etc/keystone/`.
    - `/var/lib/keystone/`.
    - `/etc/haproxy/`.
    - `/var/lib/haproxy/`.
    - `/var/lib/kubelet/`.
    - `/var/lib/kube-proxy/`.
    - `/var/lib/kubernetes/`.
    - `/var/lib/containerd/`.
    - `/var/lib/mysql/`.
    - `/var/lib/clickhouse/`.
2. Процессы:
    - `nova*`.
    - `neutron*`.
    - `glance*`.
    - `cinder*`.
    - `containerd*`.
    - `keystone`.
    - `libvirtd`.
    - `qemu*`.
    - `mysqld`.
    - `rabbitmq-server`.
    - `memcached`.
3. Файлы:
    - `.qcow2`.
    - `.raw`, `.img`, `.iso`.
    - `.pid`, `.sock`.
    - `.db`, `.sqlite`, `.log`.

