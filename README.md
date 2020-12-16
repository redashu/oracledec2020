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


