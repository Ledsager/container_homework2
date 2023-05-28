### Задание:
* запустить контейнер с ubuntu, используя механизм LXC
* ограничить контейнер 256 Мб ОЗУ и проверить, что ограничение работает
* добавить автозапуск контейнеру, перезагрузить ОС и убедиться, что контейнер действительно запустился самостоятельно
* при создании указать файл, куда записывать логи
* после перезагрузки проанализировать логи

```
lxc-create -n lxcRust -t ubuntu

root@linux-gb:/home/rust# lxc-ls -f
NAME    STATE   AUTOSTART GROUPS IPV4        IPV6 UNPRIVILEGED
lxcRust RUNNING 1         -      ------- -    false
root@linux-gb:/home/rust# lxc-attach -n lxcRust

root@lxcRust:/# free -m
               total        used        free      shared  buff/cache   available
Mem:            1024          25         996           0           2         998
Swap:              0           0           0

root@linux-gb:/var/lib# cd /var/lib/lxc
root@linux-gb:/var/lib/lxc# ls
lxcRust

root@linux-gb:/var/lib/lxc/lxcRust# ls
config  rootfs
root@linux-gb:/var/lib/lxc/lxcRust# nano config

# Distribution configuration
lxc.include = /usr/share/lxc/config/common.conf
lxc.arch = linux64

# Container specific configuration
lxc.rootfs.path = dir:/var/lib/lxc/lxcRust/rootfs
lxc.uts.name = lxcRust
lxc.cgroup2.memory.max = 256M
lxc.start.auto = 1
# Network configuration
lxc.net.0.type = veth
lxc.net.0.link = lxcbr0
lxc.net.0.flags = up
lxc.net.0.hwaddr = --------
lxc.net.0.ipv4.address = --------

root@linux-gb:/# lxc-start -n lxcRust --logfile /logfile.log
root@linux-gb:/# lxc-start -n lxcRust --logfile /logfile.log -l debug
root@linux-gb:/# cat /logfile.log

```
