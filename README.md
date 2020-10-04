# ospf
Приватные подсети 172.20.10.1/24, 172.20.20.1/24 и 172.20.30.1/24 объединены с помощью ospf

Маршруты:

![Image alt](https://github.com/Sergey-SSA/ospf/blob/main/printscreen/1.png)

![Image alt](https://github.com/Sergey-SSA/ospf/blob/main/printscreen/2.png)

![Image alt](https://github.com/Sergey-SSA/ospf/blob/main/printscreen/3.png)

Симметричный роутинг:

![Image alt](https://github.com/Sergey-SSA/ospf/blob/main/printscreen/4.png)

Поменяв вес пути в подсети 192.168.0.0/30, связывающей r1 и r2, для интерфейса 192.168.0.1 роутера r1, на , например, 1000, получим ассиметричный роутинг:
```
r1# configure  terminal 
r1(config)# interface eth2
r1(config-if)# ip ospf  cost  1000
r1(config-if)# exit
r1(config)# exit
r1# exit
[root@r1 ~]# tracepath -n 172.20.20.1
 1?: [LOCALHOST]                                         pmtu 1500
 1:  192.168.200.2                                         0.976ms 
 1:  192.168.200.2                                         0.417ms 
 2:  172.20.20.1                                           1.053ms reached
     Resume: pmtu 1500 hops 2 back 2 
```
Вернуть симметричность роутинга можно, например, повысив вес пути до 1000 в подсети 192.168.100.0/30 для интерфейса 192.168.100.1 роутера r3.
```
[root@r3 ~]# vtysh 

Hello, this is Quagga (version 0.99.22.4).
Copyright 1996-2005 Kunihiro Ishiguro, et al.

r3# configure  terminal  
r3(config)# interface  eth3
r3(config-if)# ip ospf  cost 1000
r3(config-if)# exit
r3(config)# exit
r3# exit
```
```
[root@r1 ~]# tracepath -n 172.20.20.1
 1?: [LOCALHOST]                                         pmtu 1500
 1:  172.20.20.1                                           0.615ms reached
 1:  172.20.20.1                                           0.547ms reached
     Resume: pmtu 1500 hops 1 back 1 
```
