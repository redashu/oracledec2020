# Docker & k8s

## Docker tcp socket 

```
ec2-user@ip-172-31-66-188 ~]$ cd /etc/sysconfig/
[ec2-user@ip-172-31-66-188 sysconfig]$ ls
acpid       clock     docker          i18n        man-db      network-scripts  readonly-root  rsyslog    sysstat
atd         console   docker-storage  init        modules     nfs              rpc-rquotad    run-parts  sysstat.ioconf
authconfig  cpupower  grub            irqbalance  netconsole  raid-check       rpcbind        selinux
chronyd     crond     htcacheclean    keyboard    network     rdisc            rsyncd         sshd
[ec2-user@ip-172-31-66-188 sysconfig]$ sudo vim docker
[ec2-user@ip-172-31-66-188 sysconfig]$ cat  docker
# The max number of open files for the daemon itself, and all
# running containers.  The default value of 1048576 mirrors the value
# used by the systemd service unit.
DAEMON_MAXFILES=1048576

# Additional startup options for the Docker daemon, for example:
# OPTIONS="--ip-forward=true --iptables=true"
# By default we limit the number of open files per container
OPTIONS="--default-ulimit nofile=1024:4096 -g /var/lib/oracledocker -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock"

# How many seconds the sysvinit script waits for the pidfile to appear
# when starting the daemon.
DAEMON_PIDFILE_TIMEOUT=10
[ec2-user@ip-172-31-66-188 sysconfig]$ sudo systemctl daemon-reload 
[ec2-user@ip-172-31-66-188 sysconfig]$ sudo systemctl restart docker
[ec2-user@ip-172-31-66-188 sysconfig]$ sudo netstat -ntpl  |  grep -i docker
tcp6       0      0 :::2375                 :::*                    LISTEN      5858/dockerd 

```

## COnnecting docker remote engine from mac 

```
 export DOCKER_HOST="tcp://34.233.20.87:2375"
```

## From windows powershell

```
$env:DOCKER_HOST="tcp://34.233.20.87:2375"

```
