# Docker & k8s

## Day1 rev

<img src="day1rev.png">

## Dockerfile with arg 

```
[ec2-user@ip-172-31-66-188 test]$ cat Dockerfile 
FROM centos
MAINTAINER  ashutoshh
ARG x=httpd
RUN  yum install vim $x -y
RUN  echo "Hello world"  >/var/www/html/index.html 
EXPOSE 80
ENTRYPOINT ["httpd","-DFOREGROUND"]
```

## running dockerfile 

```
 313  docker  build -t  ashu:testv1  . 
  314  docker  build --build-arg x=ftp   -t  ashu:testv1  . 
  
```

## arg variable scope is not to the container 

```
[ec2-user@ip-172-31-66-188 test]$ docker run --rm -it  --entrypoint bash   ashu:testv1  
[root@d9f7cc6afdad /]# 
[root@d9f7cc6afdad /]# env
LANG=en_US.UTF-8
HOSTNAME=d9f7cc6afdad
PWD=/
HOME=/root
TERM=xterm
SHLVL=1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
LESSOPEN=||/usr/bin/lesspipe.sh %s
_=/usr/bin/env


```

## arg and env 

```
[ec2-user@ip-172-31-66-188 test]$ cat  Dockerfile 
FROM centos
MAINTAINER  ashutoshh
ARG x=httpd
ENV y=$x
RUN  yum install vim $y -y
RUN  echo "Hello world"  >/var/www/html/index.html 
EXPOSE 80
ENTRYPOINT ["httpd","-DFOREGROUND"]


```



