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


   
## container child process

```
[ec2-user@ip-172-31-66-188 ~]$ docker  exec  -it  ashux1  sh 
/ # 
/ # 
/ # ls
bin    dev    etc    home   lib    media  mnt    opt    proc   root   run    sbin   srv    sys    tmp    usr    var
/ # uname 
Linux
/ # uname -r
4.14.203-156.332.amzn2.x86_64
/ # cat  /etc/os-release 
NAME="Alpine Linux"
ID=alpine
VERSION_ID=3.12.2
PRETTY_NAME="Alpine Linux v3.12"
HOME_URL="https://alpinelinux.org/"
BUG_REPORT_URL="https://bugs.alpinelinux.org/"

```

##

```
 44  docker  exec  ashux1  uname  
   45  docker  exec  ashux1 "uname -r"
   46  docker  exec  ashux1  ping google.com
   47  docker  ps
   48  docker  exec  ashux1 uname -r
   49  uname -r
   50  history 
   51  docker  exec  -d ashux1  ping google.com

```
   
   
## Docker image building process

<img src="build.png">

## sample python script

```
import time,subprocess

while 3 > 2:
    print("Hello world")
    noc=subprocess.getoutput("docker ps ")
    time.sleep(2)
    print("all containers ",noc)
    print("________________________")
    print("________________________")
    time.sleep(2)

```

## dockerfile

```
[ec2-user@ip-172-31-66-188 pyapp]$ cat Dockerfile 
FROM  python
# we are telling docker engine to use python as base image
# if it is not present in your system it will pull it from docker hub
MAINTAINER  ashutoshh@linux.com , ashutoshh singh 
# info about dockerfile creator
# optional step 
RUN  mkdir  /mycode
# inside container if something i want to perform 
COPY  hello.py  /mycode/hello.py
# take data from localhost and copy it to docker image during building time 
# only current location data will be considered 
WORKDIR  /mycode
# changing directory during build time 
RUN chmod +x hello.py
CMD  ["python","hello.py"]
# cmd will be used to decide the default parent process for my docker image 

```

## bulding image

```
[ec2-user@ip-172-31-66-188 pyapp]$ docker  build  -t  ashu:pyv1  . 
Sending build context to Docker daemon  3.584kB
Step 1/7 : FROM  python
latest: Pulling from library/python
6c33745f49b4: Pull complete 
c87cd3c61e27: Pull complete 
05a3c799ec37: Pull complete 
a61c38f966ac: Pull complete 
c2dd6d195b68: Pull complete 
29b9446ae7bd: Pull complete 
09cf96c794f9: Pull complete 
f674fd97fba7: Pull complete 
9c7f9d05b1c1: Pull complete 
Digest: sha256:54f6f8b98d982f5b88f22f8d0cbed67a762d7b1ceffcc9a0dedfc30a639dffa8
Status: Downloaded newer image for python:latest
 ---> 2770b69c10e1
Step 2/7 : MAINTAINER  ashutoshh@linux.com , ashutoshh singh
 ---> Running in bdaf3497ef11
Removing intermediate container bdaf3497ef11
 ---> 7dde1f21818a
Step 3/7 : RUN  mkdir  /mycode
 ---> Running in 716ce9c3c1e2
Removing intermediate container 716ce9c3c1e2
 ---> 9bb721b372b2
Step 4/7 : COPY  hello.py  /mycode/hello.

```

## creating container 

```
[ec2-user@ip-172-31-66-188 pyapp]$ docker  run -it -d  --name  ashupc1  ashu:pyv1 
c4363349751d339a7bff916c00318f20c84e898e6381b5e3962e70171240fa20
[ec2-user@ip-172-31-66-188 pyapp]$ docker  ps 
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
c4363349751d        ashu:pyv1           "python hello.py"   4 seconds ago       Up 3 seconds                            ashupc1

```


# container registry 

<img src="cr.png">

## image name reality 

<img src="imgname.png">


## pushing docker image to docker hub 

```
 184  docker  login  -u dockerashu 
  185  docker  tag  ashu:pyv1   dockerashu/ashu:pyv1
  186  docker push dockerashu/ashu:pyv1

```
   
# nginx based dockerfile

```
[ec2-user@ip-172-31-66-188 htmlapp]$ cat  nginx.dockerfile 
FROM  nginx
MAINTAINER  ashutoshh
WORKDIR  /usr/share/nginx/html 
# above directory is documentroot of nginx webserver 
COPY  .  .
# first . is source from docker HOST and second . is target location 
EXPOSE  80 
# nginx is having 80 port by default 

```

===


```
[ec2-user@ip-172-31-66-188 htmlapp]$ cat  .dockerignore 
nginx.dockerfile
.git
*.md
LICENSE
.dockerignore

```


## building image

```
[ec2-user@ip-172-31-66-188 htmlapp]$ docker build  -t  dockerashu/nginx:v0011  -f  nginx.dockerfile  .
Sending build context to Docker daemon  63.49kB
Step 1/5 : FROM  nginx
 ---> ae2feff98a0c
Step 2/5 : MAINTAINER  ashutoshh
 ---> Running in 0d81238d0232
Removing intermediate container 0d81238d0232
 ---> 204d34da36c4
Step 3/5 : WORKDIR  /usr/share/nginx/html
 ---> Running in ca2a29b9933a
Removing intermediate container ca2a29b9933a
 ---> b7122f4104e4
Step 4/5 : COPY  .  .
 ---> ecd7dd43be84
Step 5/5 : EXPOSE  80
 ---> Running in 08f906cb66b7
Removing intermediate container 08f906cb66b7
 ---> 72b3326f4e58
Successfully built 72b3326f4e58
Successfully tagged dockerashu/nginx:v0011

```
## creating container

```
docker  run -d --name ashungc1  -p 1234:80   72b3326f4e58 
```

## docker assignment report

```
192  docker  cp ashuc1:/aa.txt  ashuc2:/aa.txt
  193  docker  cp ashuc1:/aa.txt  .
  194  docker  cp ashuc1:/bb..txt  .
  195  ls
  196  docker  cp ashuc1:/bb.txt  .
  197  ls
  198  docker  cp *.txt  ashuc2:/
  199  docker  cp aa.txt  ashuc2:/aa.txt
  200  docker  cp bb.txt  ashuc2:/bb.txt
  
  ```
  
  # HTTPD based dockerfile
  
  ```
  [ec2-user@ip-172-31-66-188 app1]$ cat  Dockerfile 
FROM  centos
MAINTAINER  ashutoshh
RUN  yum install  httpd -y
WORKDIR  /var/www/html/
ADD . . 
# add & copy both are same 
EXPOSE 80
CMD  ["/usr/sbin/httpd", "-DFOREGROUND"]

```

## creating container

```
[ec2-user@ip-172-31-66-188 app1]$ docker run -d --name ashux1 -p 1122:80  ashuapache:httpdv2  
10d67e329cb76017008dcc0850fa3c242c232c4cd726f5808604e058d7e857af
[ec2-user@ip-172-31-66-188 app1]$ 
[ec2-user@ip-172-31-66-188 app1]$ 
[ec2-user@ip-172-31-66-188 app1]$ docker  ps
CONTAINER ID        IMAGE                COMMAND                  CREATED             STATUS              PORTS                  NAMES
10d67e329cb7        ashuapache:httpdv2   "/usr/sbin/httpd -DF…"   7 seconds ago       Up 6 seconds        0.0.0.0:1122->80/tcp   ashux1
f6f5bdfedf37        b5bd76b7e95a         "/usr/sbin/httpd -DF…"   19 seconds ago      Up 18 seconds       0.0.0.0:6524->80/tcp   amarhttpd
[ec2-user@ip-172-31-66-188 app1]$ curl http://ipinfo.io/json 
{
  "ip": "34.233.20.87",
  "hostname": "ec2-34-233-20-87.compute-1.amazonaws.com",
  "city": "Virginia Beach",
  "region": "Virginia",
  "country": "US",
  "loc": "36.7978,-76.1759",
  "org": "AS14618 Amazon.com, Inc.",
  "postal": "23464",
  "timezone": "America/New_York",
  "readme": "https://ipinfo.io/missingauth"
  
  ```
  
  ## Docker restart policy 
  
  [link] ('https://docs.docker.com/config/containers/start-containers-automatically/')
  
  ## restart policy 
  
  ```
  [ec2-user@ip-172-31-66-188 ~]$ docker run -d --name xx11 --restart always -p 9900:80 ashuapache:httpdv2 
22ff804d42a19cb75870f84ea89c559e18e0536149d6d775b61fa6e09790f1e7
[ec2-user@ip-172-31-66-188 ~]$ docker  update  ashux1  --restart always
ashux1


```

  
