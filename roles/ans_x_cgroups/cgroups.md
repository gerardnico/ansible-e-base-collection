# Cgroups
## About
`Cgroups` is a group of Linux processes with optional resource isolation, accounting and limits.

Cgroups is a resource management system
 to be able to manage the restrained the resources of each process
 (Linux kernel feature)

## Version

```bash
stat -fc %T /sys/fs/cgroup/
```
```bash
cgroup2fs # cgroups v2
tmpfs # cgroups v1
```

It's a file system
```bash
grep cgroup /proc/filesystems
```
```
nodev   cgroup
nodev   cgroup2
```

### See Cgroups hierarchy

```bash
systemctl status
#or
systemd-cgls
```
```
CGroup: /
├─init.scope
│ └─1 /usr/lib/systemd/systemd --switched-root --system --deserialize=31
├─system.slice
│ ├─NetworkManager.service
│ │ └─666 /usr/sbin/NetworkManager --no-daemon
.......................
```

## Conf
The configuration as seen in the service
  `/etc/cgconfig.conf`

Persistent group configuration: If you want your cgroups to be created at boot,
you need to define them in `/etc/cgconfig.conf`.


### Cpu
#### Shares

* cpu.shares with cgroupv1. 1024 shares for every process
* cpu.weight with cgroupv2. 100 weight for every process

`cpu.shares`: By default, all groups have 1024 CPU shares, giving 512 means 50%
  CPU priority ("shares") By default all groups have 1024 shares. A group with 100 shares will get a ~10% portion of the CPU time:
`cpu.weight`: A single value file which exists on non-root cgroups,
used to set a proportional bandwidth limit.
The default is 100. The weight is in the range (1, 10000)
[Ref](https://facebookmicrosites.github.io/cgroup2/docs/cpu-controller.html)

## Command



### cgcreate

```bash
cgcreate -t uid:gid -a uid:gid -g subsystems:path
# example
cgcreate -g cpu:/Browsers
```

Check the creation

```bash
lscgroup | grep groupName
```
```txt
cpuset,cpu,io,memory,hugetlb,pids,rdma,misc:/cpu_50
```

### cgset

```bash
cgset -r parameter=value path_to_cgroup

# example
cgset -r cpu.shares=1024 Browsers
cgset -r cpu.shares=2048 Multimedia
```


### cgclassify

To move a process into a cgroup, you use the command cgclassify

```bash
cgclassify -g subsystems:path_to_cgroup pidlist
# example (where 124 is the PID)
cgclassify -g cpu:Browsers 1234
```

### Other command
```
cgdelete
cgexec
cgget
cgset
cgsnapshot
lscgroup
```

# List of subsystems

A subsystem is a category that manages the resources.
It set limits, restrict access, or define allocations for each subsystem.

* cpu : this subsystem uses the scheduler to provide cgroup processes access to the CPU.
* cpuacct : this subsystem generates automatic reports on CPU resources used by processes in a cgroup.
* memory : this subsystem sets limits on memory use by processes in a cgroup, and generates automatic reports on memory resources used by those processes.


```bash
cat /proc/cgroups
# or
ls /sys/fs/cgroup/
```
```
#subsys_name    hierarchy       num_cgroups     enabled
cpuset  8       1       1
cpu     2       66      1
cpuacct 2       66      1
memory  6       66      1
devices 7       66      1
freezer 11      1       1
net_cls 4       1       1
blkio   10      66      1
perf_event      9       1       1
hugetlb 3       1       1
pids    5       66      1
net_prio        4       1       1
```


## Doc


* https://drill.apache.org/docs/configuring-cgroups-to-control-cpu-usage/
* https://wiki.archlinux.org/title/Cgroups

* https://facebookmicrosites.github.io/cgroup2/docs/cpu-controller.html

* Kernel https://www.kernel.org/doc/html/latest/admin-guide/cgroup-v2.html
For the Fedora Vps with the Linux Version
The doc is https://www.kernel.org/doc/html/v6.4/admin-guide/cgroup-v1/cgroups.html
