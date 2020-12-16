# Docker & k8s


# Hypervison in action 

<img src="hv.png">

# Container model engines

<img src="cre.png">

## containers

<img src="c1.png">

## docker supported kernel 


<img src="dk.png">

## docker installation using docker desktop

<img src="dinstall.png">

## Docker desktop for windows 10

[docker desktop] ('https://hub.docker.com/editions/community/docker-ce-desktop-windows/')


## docker desktop for mac 

[dd] ('https://hub.docker.com/editions/community/docker-ce-desktop-mac')



## docker ce installation in linux directly 

[install docker] ('https://docs.docker.com/engine/install/centos/')

## docker installation on linux

```
[root@ip-172-31-66-188 ~]# yum install docker 
Failed to set locale, defaulting to C
Loaded plugins: extras_suggestions, langpacks, priorities, update-motd
amzn2-core                                                         | 3.7 kB  00:00:00     
Resolving Dependencies
--> Running transaction check
---> Package docker.x86_64 0:19.03.13ce-1.amzn2 will be installed
--> Processing Dependency: runc >= 1.0.0 for package: docker-19.03.13ce-1.amzn2.x86_64
--> Processing Dependency: containerd >= 1.3.2 for package: docker-19.03.13ce-1.amzn2.x86_64

```

## starting docker 

```
[root@ip-172-31-66-188 ~]# systemctl  start  docker 
[root@ip-172-31-66-188 ~]# systemctl enable  docker 
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
[root@ip-172-31-66-188 ~]# systemctl  status  docker 
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2020-12-16 05:21:13 UTC; 14s ago
     Docs: https://docs.docker.com
 Main PID: 32722 (dockerd)
   CGroup: /system.slice/docker.service
           └─32722 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.so...

Dec 16 05:21:12 ip-172-31-66-188.ec2.internal dockerd[32722]: time="2020-12-16T05:21:12...
Dec 16 05:21:12 ip-172-31-66-188.ec2.internal dockerd[32722]: time="2020-12-16T05:21:12...
Dec 16 05:21:12 ip-172-31-66-188.ec2.internal dockerd[32722]: time="2020-12-16T05:21:12...
Dec 16 05:21:12 ip-172-31-66-188.ec2.internal

```

## docker architecture 

<img src="darch.png">

## adding users in docker group

```
[root@ip-172-31-66-188 ~]# for  i  in  `ls  /home`
> do
> usermod -aG docker $i
> done

```

## connecting with docker engine

```
[ec2-user@ip-172-31-66-188 ~]$ docker version 
Client:
 Version:           19.03.13-ce
 API version:       1.40
 Go version:        go1.13.15
 Git commit:        4484c46
 Built:             Mon Oct 12 18:51:20 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server:
 Engine:
  Version:          19.03.13-ce
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       4484c46
  Built:            Mon Oct 12 18:51:50 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:

```

## summary so far

<img src="sum.png">


## image registry 

<img src="reg.png">


## searching image on docker hub 

```
[ec2-user@ip-172-31-66-188 ~]$ docker  search  mysql 
NAME                              DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   10269               [OK]                
mariadb                           MariaDB is a community-developed fork of MyS…   3794                [OK]                
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   749                                     [OK]
percona                           Percona Server is a fork of the MySQL relati…   515                 [OK]                
centos/mysql-57-centos7           MySQL 5.7 SQL database server                   86                                      
mysql/mysql-cluster               

```

## checking images

```
[ec2-user@ip-172-31-66-188 ~]$ docker  images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE


```

## container creation process

<img src="contp.png">

## first container creation 

```
[ec2-user@ip-172-31-66-188 ~]$ docker  run  alpine:latest  ping fb.com 
PING fb.com (31.13.66.35): 56 data bytes
64 bytes from 31.13.66.35: seq=0 ttl=51 time=0.719 ms
64 bytes from 31.13.66.35: seq=1 ttl=51 time=0.757 ms
64 bytes from 31.13.66.35: seq=2 ttl=51 time=0.776 ms
64 bytes from 31.13.66.35: seq=3 ttl=51 time=0.736 ms

```

## container with background process

```
docker  run  --name  ashuc1 -d  alpine  ping 8.8.8.8
```

## checking running container list 

```
[ec2-user@ip-172-31-66-188 ~]$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
8d1163736bb8        alpine              "ping 8.8.8.8"      About a minute ago   Up About a minute                       raj1
9e8e95e870d1        alpine              "ping 8.8.8.8"      2 minutes ago        Up 2 minutes                            amardc1
84b09117c6ed        alpine              "ping google.com"   2 minutes ago        Up 2 minutes                            ramya_test
3347432234bc        alpine              "ping 8.8.8.8"      3 minutes ago        Up 2 minutes                            som1
b01dab207aa8        alpine              "ping google.com"   3 minutes ago        Up 3

```
## checking output of container

```
50  docker  logs  ashuc1 
   51  docker  logs -f  ashuc1 
   
```


## more docker operations

```
 14  docker   stop  ashuc1
   15  docker  ps
   16  docker  ps -a
   17  history 
   18  docker  start  ashuc1
   19  docker  ps
   20  docker  kill  ashuc1
   21  docker  ps
   22  docker  start  ashuc1
   23  docker  ps

```

## cleaning up all the containers

```
[ec2-user@ip-172-31-66-188 ~]$ docker kill  $(docker ps  -q)
5b961130d22f
4f9733727187
8d1163736bb8
f426c78d48f3
[ec2-user@ip-172-31-66-188 ~]$ docker  ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[ec2-user@ip-172-31-66-188 ~]$ docker rm  $(docker ps  -q -a)
5b961130d22f
4f9733727187
8d1163736bb8
9e8e95e870d1
3347432234bc
b01dab207aa8
265c771eac65
f426c78d48f3
22212073cac9
44db9a7e3796
89c0ed5a86ce
[ec2-user@ip-172-31-66-188 ~]$ docker  ps  -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[ec2-user@ip-172-31-66-188 ~]$ 


```


   
