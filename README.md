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

## DB in container

```
[ec2-user@ip-172-31-66-188 ~]$ docker run -d --name ashudb -e MYSQL_ROOT_PASSWORD=oracle123  mysql 

==
[ec2-user@ip-172-31-66-188 ~]$ docker  exec  -it  ashudb bash 
root@9d4543ad32f4:/# cat  /etc/os-release 
PRETTY_NAME="Debian GNU/Linux 10 (buster)"
NAME="Debian GNU/Linux"
VERSION_ID="10"
VERSION="10 (buster)"
VERSION_CODENAME=buster
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
root@9d4543ad32f4:/# mysql -u root  -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.22 MySQL Community Server - GPL

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 

---
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)


```
## adminer and mysql 

```
[ec2-user@ip-172-31-66-188 ashuapp]$ cat docker-compose.yml 
# Use root/example as user/password credentials
version: '3.8'

services:

  ashudb:
    image: mysql
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: oracle

  ashuadminer:
    image: adminer
    restart: always
    depends_on:
     - ashudb
    ports:
      - 1234:8080
      
 ```
 
# COntainer problems

<img src="cp.png">


## container orchestration tool

<img src="or.png">


