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

# multi app 

## dockerfile 

```
[ec2-user@ip-172-31-66-188 multiapp]$ cat Dockerfile 
FROM oraclelinux:7.9
MAINTAINER ashutoshh@linux.com
ARG software=httpd
RUN yum install $software  -y
ENV deploy=app
RUN mkdir /client  /client/app1  /client/app2  /client/app3
COPY app1  /client/app1/
COPY app2  /client/app2/
COPY app3  /client/app3/
COPY startapp.sh  /client/startapp.sh
WORKDIR  /client
RUN chmod +x startapp.sh
EXPOSE 80
ENTRYPOINT  ["./startapp.sh"] 

```

## shell script 

```
[ec2-user@ip-172-31-66-188 multiapp]$ cat startapp.sh 
#!/bin/bash 


if  [ "$deploy"  ==  "app1" ]
then
	cp -rf  /client/app1/*    /var/www/html/
	httpd -DFOREGROUND 

elif  [ "$deploy"  ==  "app2" ]
then
	cp -rf  /client/app2/*    /var/www/html/
	httpd -DFOREGROUND 


elif  [ "$deploy"  ==  "app3" ]
then
	cp -rf  /client/app3/*    /var/www/html/
	httpd -DFOREGROUND 


else 

	echo "You variable is not correct"    >/var/www/html/index.html 
	httpd -DFOREGROUND 

fi

```


## building docker image

```
 docker build  -t  dockerashu/oracleweb:appv001 .
 ```
 
 ## deployment of containers
 
 ```
  368  docker run -d --name ashuxx12 -p 8811:80 -e deploy=app1 dockerashu/oracleweb:appv001
  369  docker  ps
  370  curl http://ipinfo.io/json 
  371  docker run -d --name ashuxx13 -p 8800:80 -e deploy=app2 dockerashu/oracleweb:appv001
  372  docker run -d --name ashuxx13 -p 8888:80 -e deploy=app3 dockerashu/oracleweb:appv001
  373  docker run -d --name ashuxx15 -p 8888:80 -e deploy=app3 dockerashu/oracleweb:appv001
  
  ```
  
  ## memory and cpu limits
  
  ```
  143  docker  stats   ashuxx15
  144  docker  stats   
  145  history 
  146  docker  update  ashuxx15 --memory=100M 
  147  docker  update  --help
  148  free  -m
  149  history 
  150  docker  run -d --name cc33 --memory 400m alpine ping fb.com 
  151  docker  stats cc33
  152  history 
[raj@ip-172-31-66-188 multiapp]$ docker  run -d --name cc33 --memory 400m --cpu-share=30 alpine ping fb.com 

```

# Docker networking 

<img src="dnet.png">

```
[ec2-user@ip-172-31-66-188 ~]$ docker  network  ls
NETWORK ID          NAME                DRIVER              SCOPE
e7932d9006fa        bridge              bridge              local
c090dd9621f7        host                host                local
2193e4221c29        none                null                local
[ec2-user@ip-172-31-66-188 ~]$ ifconfig  docker0
docker0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::42:e6ff:fed8:3d5a  prefixlen 64  scopeid 0x20<link>
        ether 02:42:e6:d8:3d:5a  txqueuelen 0  (Ethernet)
        RX packets 40023  bytes 9554173 (9.1 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 61162  bytes 664981507 (634.1 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

## docker bridge details 

```
[ec2-user@ip-172-31-66-188 ~]$ docker network inspect bridge 
[
    {
        "Name": "bridge",
        "Id": "e7932d9006fa64b1ee2fea7606a7cbae6072470652b62c18168597c1189431d4",
        "Created": "2020-12-17T03:55:48.674034918Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }

```
## creating new bridge

```
[ec2-user@ip-172-31-66-188 ~]$ docker network create  ashubr1 
5e6007b32680db9466a7f4d4c656706adc83b96d8ea1a06f770538db34ef863c
[ec2-user@ip-172-31-66-188 ~]$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
5e6007b32680        ashubr1             bridge              local
e7932d9006fa        bridge              bridge              local
c090dd9621f7        host                host                local

```
## inspecting 

```
[ec2-user@ip-172-31-66-188 ~]$ docker network inspect   ashubr1 
[
    {
        "Name": "ashubr1",
        "Id": "5e6007b32680db9466a7f4d4c656706adc83b96d8ea1a06f770538db34ef863c",
        "Created": "2020-12-17T07:16:51.278233803Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }

```

## attaching container in diff bridge

```
[ec2-user@ip-172-31-66-188 ~]$ docker  run -it --rm --network ashubr1 alpine sh 
/ # ifconfig 
eth0      Link encap:Ethernet  HWaddr 02:42:AC:12:00:02  
          inet addr:172.18.0.2  Bcast:172.18.255.255  Mask:255.255.0.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:11 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:962 (962.0 B)  TX bytes:0 (0.0 B)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:0 errors:0 dropped:0 overruns:0 frame:0
          TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:0 (0.0 B)  TX bytes:0 (0.0 B)

/ # ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2): 56 data bytes
^C
--- 172.17.0.2 ping statistics ---
3 packets transmitted, 0 packets received, 100% packet loss
/ # ping 172.17.0.3
PING 172.17.0.3 (172.17.0.3): 56 data bytes
^C
--- 172.17.0.3 ping statistics ---
2 packets transmitted, 0 packets received, 100% packet loss

```


## container with static ip 

```
[ec2-user@ip-172-31-66-188 ~]$ docker network create  ashubr2  --subnet 192.168.1.0/24 
efae21937a38a6ef298392257d0fff292803a75a81f5f3547f1e667837933c95
[ec2-user@ip-172-31-66-188 ~]$ 
[ec2-user@ip-172-31-66-188 ~]$ 
[ec2-user@ip-172-31-66-188 ~]$ docker run -d --name xc1 --network ashubr2 --ip 192.168.1.100 alpine ping fb.com 
8ad6053d2cb0315c8285b0f173bc60ee15f98ed6db7b566c9242eb850e41bac9
[ec2-user@ip-172-31-66-188 ~]$ docker run -d --name xc2 --network ashubr2 alpine ping fb.com 
7496b70cc098ce14cdb9ea3365ba1ebbdd29639f3eba24e8d4c85a08ed6c2ecb
[ec2-user@ip-172-31-66-188 ~]$ docker  exec -it xc2 sh 
/ # ifconfig 
eth0      Link encap:Ethernet  HWaddr 02:42:C0:A8:01:02  
          inet addr:192.168.1.2  Bcast:192.168.1.255  Mask:255.255.255.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:25 errors:0 dropped:0 overruns:0 frame:0
          TX packets:18 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:2218 (2.1 KiB)  TX bytes:1588 (1.5 KiB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:4 errors:0 dropped:0 overruns:0 frame:0
          TX packets:4 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:252 (252.0 B)  TX bytes:252 (252.0 B)

/ # ping 192.168.1.100
PING 192.168.1.100 (192.168.1.100): 56 data bytes
64 bytes from 192.168.1.100: seq=0 ttl=255 time=0.127 ms
64 bytes from 192.168.1.100: seq=1 ttl=255 time=0.125 ms
^C
--- 192.168.1.100 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 0.125/0.126/0.127 ms
/ # 
/ # ping xc1
PING xc1 (192.168.1.100): 56 data bytes
64 bytes from 192.168.1.100: seq=0 ttl=255 time=0.089 ms
64 bytes from 192.168.1.100: seq=1 ttl=255 time=0.116 ms
64 bytes from 192.168.1.100: seq=2 ttl=255 time=0.116 ms
^C
--- xc1 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.089/0.107/0.116 ms

```
