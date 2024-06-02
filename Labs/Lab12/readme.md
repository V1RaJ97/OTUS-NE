# Лабораторная работа - Настройка NAT для IPv4
## Топология
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/a36ec720485414844fc9c5b5d58db2430bccea01/Labs/Lab12/%D0%A2%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F.png)

## Таблица адресации
| Устройство |     Интерфейс    |     IP-адрес    |  Маска подсети  |
|:----------:|:----------------:|:---------------:|:---------------:|
|     R1     |      G0/0/0      | 209.165.200.230 | 255.255.255.248 |
|     R1     |      G0/0/1      |   192.168.1.1   |  255.255.255.0  |
|     R2     |      G0/0/0      | 209.165.200.225 | 255.255.255.248 | 
|     R2     |       Lo1        |  209.165.200.1  | 255.255.255.224 |
|     S1     |       VLAN 1     |  192.168.1.11   |  255.255.255.0  |
|     S2     |       VLAN 1     |  192.168.1.12   |  255.255.255.0  |
|    PC-A    |       NIC        |  192.168.1.2    |  255.255.255.0  |
|    PC-B    |       NIC        |  192.168.1.3    |  255.255.255.0  |
## Цели
1. Создание сети и настройка основных параметров устройства
2. Настройка и проверка NAT для IPv4
3. Настройка и проверка PAT для IPv4
4. Настройка и проверка статического NAT для IPv4.

## Часть 1. Создание сети и настройка основных параметров устройства
### Финальная конфигурация маршрутизаторов
```
R1#sh running-config 
Building configuration...

Current configuration : 898 bytes
!
version 16.6.4
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname R1
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
ip cef
no ipv6 cef
!
no ip domain-lookup
!
spanning-tree mode pvst
!
interface GigabitEthernet0/0/0
 ip address 209.165.200.230 255.255.255.248
 duplex auto
 speed auto
!
interface GigabitEthernet0/0/1
 ip address 192.168.1.1 255.255.255.0
 duplex auto
 speed auto
!
interface GigabitEthernet0/0/2
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface Vlan1
 no ip address
 shutdown
!
ip classless
ip route 0.0.0.0 0.0.0.0 209.165.200.225 
!
ip flow-export version 9
!
banner motd ^C Unauthorized access is strictly prohibited ^C
!
line con 0
 password 7 0822455D0A16
 logging synchronous
 login
!
line aux 0
!
line vty 0 4
 password 7 0822455D0A16
 logging synchronous
 login
end
```
```
R2#sh running-config 
Building configuration...

Current configuration : 996 bytes
!
version 16.6.4
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname R2
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
ip cef
no ipv6 cef
!
no ip domain-lookup
!
spanning-tree mode pvst
!
interface Loopback1
 ip address 209.165.200.1 255.255.255.224
!
interface GigabitEthernet0/0/0
 ip address 209.165.200.225 255.255.255.248
 duplex auto
 speed auto
!
interface GigabitEthernet0/0/1
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface GigabitEthernet0/0/2
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface Vlan1
 no ip address
 shutdown
!
ip classless
ip route 0.0.0.0 0.0.0.0 209.165.200.230
!
ip flow-export version 9
!
banner motd ^C Unauthorized access is strictly prohibited ^C
!
line con 0
 password 7 0822455D0A16
 logging synchronous
 login
!
line aux 0
!
line vty 0 4
 password 7 0822455D0A16
 logging synchronous
 login
end
```
### Финальная конфигурация коммутаторов
```
S1#sh running-config 
Building configuration...

Current configuration : 1534 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname S1
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
no ip domain-lookup
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
!
interface FastEthernet0/2
!
interface FastEthernet0/3
 shutdown
!
interface FastEthernet0/4
 shutdown
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
 shutdown
!
interface FastEthernet0/8
 shutdown
!
interface FastEthernet0/9
 shutdown
!
interface FastEthernet0/10
 shutdown
!
interface FastEthernet0/11
 shutdown
!
interface FastEthernet0/12
 shutdown
!
interface FastEthernet0/13
 shutdown
!
interface FastEthernet0/14
 shutdown
!
interface FastEthernet0/15
 shutdown
!
interface FastEthernet0/16
 shutdown
!
interface FastEthernet0/17
 shutdown
!
interface FastEthernet0/18
 shutdown
!
interface FastEthernet0/19
 shutdown
!
interface FastEthernet0/20
 shutdown
!
interface FastEthernet0/21
 shutdown
!
interface FastEthernet0/22
 shutdown
!
interface FastEthernet0/23
 shutdown
!
interface FastEthernet0/24
 shutdown
!
interface GigabitEthernet0/1
 shutdown
!
interface GigabitEthernet0/2
 shutdown
!
interface Vlan1
 ip address 192.168.1.11 255.255.255.0
!
banner motd ^C Unauthorized access is strictly prohibited ^C
!
line con 0
 password 7 0822455D0A16
 logging synchronous
 login
!
line vty 0 4
 password 7 0822455D0A16
 logging synchronous
 login
line vty 5 15
 login
end
```
```
S2#show running-config 
Building configuration...

Current configuration : 1554 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname S2
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
no ip domain-lookup
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
!
interface FastEthernet0/2
 shutdown
!
interface FastEthernet0/3
 shutdown
!
interface FastEthernet0/4
 shutdown
!
interface FastEthernet0/5
 shutdown
!
interface FastEthernet0/6
 shutdown
!
interface FastEthernet0/7
 shutdown
!
interface FastEthernet0/8
 shutdown
!
interface FastEthernet0/9
 shutdown
!
interface FastEthernet0/10
 shutdown
!
interface FastEthernet0/11
 shutdown
!
interface FastEthernet0/12
 shutdown
!
interface FastEthernet0/13
 shutdown
!
interface FastEthernet0/14
 shutdown
!
interface FastEthernet0/15
 shutdown
!
interface FastEthernet0/16
 shutdown
!
interface FastEthernet0/17
 shutdown
!
interface FastEthernet0/18
!
interface FastEthernet0/19
 shutdown
!
interface FastEthernet0/20
 shutdown
!
interface FastEthernet0/21
 shutdown
!
interface FastEthernet0/22
 shutdown
!
interface FastEthernet0/23
 shutdown
!
interface FastEthernet0/24
 shutdown
!
interface GigabitEthernet0/1
 shutdown
!
interface GigabitEthernet0/2
 shutdown
!
interface Vlan1
 ip address 192.168.1.12 255.255.255.0
!
banner motd ^C Unauthorized access is strictly prohibited ^C
!
line con 0
 password 7 0822455D0A16
 logging synchronous
 login
!
line vty 0 4
 password 7 0822455D0A16
 logging synchronous
 login
line vty 5 15
 login
end
```
## Настройка и проверка NAT для IPv4.
### Настройка NAT на R1, используя пул из трех адресов 209.165.200.226-209.165.200.228
```
R1(config)#access-list 1 permit 192.168.1.0 0.0.0.255
R1(config)#ip nat pool PUBLIC_ACCESS 209.165.200.226 209.165.200.228 netmask 255.255.255.248
R1(config)#ip nat inside source list 1 pool PUBLIC_ACCESS
R1(config)#int g0/0/1
R1(config-if)#ip nat inside
R1(config-if)#exit
R1(config)#int g0/0/0
R1(config-if)#ip nat outside
R1(config-if)#exit
```
#### Ping с PC-B
```
Pinging 209.165.200.1 with 32 bytes of data:

Reply from 209.165.200.1: bytes=32 time=2ms TTL=254
Reply from 209.165.200.1: bytes=32 time<1ms TTL=254
Reply from 209.165.200.1: bytes=32 time<1ms TTL=254
Reply from 209.165.200.1: bytes=32 time=7ms TTL=254
```
```
R1#show ip nat translations
Pro  Inside global     Inside local       Outside local      Outside global
icmp 209.165.200.226:57192.168.1.3:57     209.165.200.1:57   209.165.200.1:57
icmp 209.165.200.226:58192.168.1.3:58     209.165.200.1:58   209.165.200.1:58

Внутренний локальный адрес был транслирован в 209.165.200.226 
```
##### Вопросы
```
Вопрос: Во что был транслирован внутренний локальный адрес PC-B?
Ответ: Внутренний локальный адрес был транслирован в 209.165.200.226 

Вопрос: Какой тип адреса NAT является переведенным адресом?
Ответ: Dynamic NAT
```
#### Ping с PC-A
```
C:\>ping 209.165.200.1

Pinging 209.165.200.1 with 32 bytes of data:

Reply from 209.165.200.1: bytes=32 time<1ms TTL=254
Reply from 209.165.200.1: bytes=32 time<1ms TTL=254
Reply from 209.165.200.1: bytes=32 time<1ms TTL=254
Reply from 209.165.200.1: bytes=32 time<1ms TTL=254
```
```
R1#show ip nat translations
Pro  Inside global     Inside local       Outside local      Outside global
icmp 209.165.200.226:10192.168.1.2:10     209.165.200.1:10   209.165.200.1:10
icmp 209.165.200.226:11192.168.1.2:11     209.165.200.1:11   209.165.200.1:11
icmp 209.165.200.226:12192.168.1.2:12     209.165.200.1:12   209.165.200.1:12
icmp 209.165.200.226:9 192.168.1.2:9      209.165.200.1:9    209.165.200.1:9
```
#### Ping с S1
```
S1#ping 209.165.200.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 209.165.200.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
```
```
R1#show ip nat translations
Pro  Inside global     Inside local       Outside local      Outside global
icmp 209.165.200.226:21192.168.1.11:21    209.165.200.1:21   209.165.200.1:21
icmp 209.165.200.226:22192.168.1.11:22    209.165.200.1:22   209.165.200.1:22
icmp 209.165.200.226:23192.168.1.11:23    209.165.200.1:23   209.165.200.1:23
icmp 209.165.200.226:24192.168.1.11:24    209.165.200.1:24   209.165.200.1:24
icmp 209.165.200.226:25192.168.1.11:25    209.165.200.1:25   209.165.200.1:25
```
#### Ping с S2
```
S2#ping 209.165.200.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 209.165.200.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
```
```
R1#show ip nat translations
Pro  Inside global     Inside local       Outside local      Outside global
icmp 209.165.200.226:31192.168.1.11:31    209.165.200.1:31   209.165.200.1:31
icmp 209.165.200.226:32192.168.1.11:32    209.165.200.1:32   209.165.200.1:32
icmp 209.165.200.226:33192.168.1.11:33    209.165.200.1:33   209.165.200.1:33
icmp 209.165.200.226:34192.168.1.11:34    209.165.200.1:34   209.165.200.1:34
icmp 209.165.200.226:35192.168.1.11:35    209.165.200.1:35   209.165.200.1:35
icmp 209.165.200.227:63192.168.1.3:63     209.165.200.1:63   209.165.200.1:63
icmp 209.165.200.227:64192.168.1.3:64     209.165.200.1:64   209.165.200.1:64
icmp 209.165.200.227:65192.168.1.3:65     209.165.200.1:65   209.165.200.1:65
icmp 209.165.200.227:66192.168.1.3:66     209.165.200.1:66   209.165.200.1:66
icmp 209.165.200.227:67192.168.1.3:67     209.165.200.1:67   209.165.200.1:67
icmp 209.165.200.227:68192.168.1.3:68     209.165.200.1:68   209.165.200.1:68
icmp 209.165.200.227:69192.168.1.3:69     209.165.200.1:69   209.165.200.1:69
icmp 209.165.200.227:70192.168.1.3:70     209.165.200.1:70   209.165.200.1:70
icmp 209.165.200.228:25192.168.1.2:25     209.165.200.1:25   209.165.200.1:25
icmp 209.165.200.228:26192.168.1.2:26     209.165.200.1:26   209.165.200.1:26
icmp 209.165.200.228:27192.168.1.2:27     209.165.200.1:27   209.165.200.1:27
icmp 209.165.200.228:28192.168.1.2:28     209.165.200.1:28   209.165.200.1:28
```
```
R1#show ip nat translations verbose
                            ^
% Invalid input detected at '^' marker.
```
## Часть 3. Настройка и проверка PAT для IPv4.
### Настройка PAT
```
R1(config)#no ip nat inside source list 1 pool PUBLIC_ACCESS 
R1(config)#ip nat inside source list 1 pool PUBLIC_ACCESS overload 
```
#### Ping с PC-B
```
C:\>ping 209.165.200.1

Pinging 209.165.200.1 with 32 bytes of data:

Reply from 209.165.200.1: bytes=32 time<1ms TTL=254
Reply from 209.165.200.1: bytes=32 time=9ms TTL=254
Reply from 209.165.200.1: bytes=32 time<1ms TTL=254
Reply from 209.165.200.1: bytes=32 time<1ms TTL=254
```
```
R1#show ip nat translations
Pro  Inside global     Inside local       Outside local      Outside global
icmp 209.165.200.226:10192.168.1.3:10     209.165.200.1:10   209.165.200.1:10
icmp 209.165.200.226:11192.168.1.3:11     209.165.200.1:11   209.165.200.1:11
icmp 209.165.200.226:9 192.168.1.3:9      209.165.200.1:9    209.165.200.1:9
```
#### Вопросы
```
Вопрос: Во что был транслирован внутренний локальный адрес PC-B?
Ответ: Внутренний локальный адрес был транслирован в 209.165.200.226:10

Вопрос: Какой тип адреса NAT является переведенным адресом?
Ответ: Dynamic NAT
```
#### Ping с PC-A
```
C:\>ping 209.165.200.1

Pinging 209.165.200.1 with 32 bytes of data:

Reply from 209.165.200.1: bytes=32 time=2ms TTL=254
Reply from 209.165.200.1: bytes=32 time<1ms TTL=254
Reply from 209.165.200.1: bytes=32 time=1ms TTL=254
Reply from 209.165.200.1: bytes=32 time<1ms TTL=254
```
```
R1#show ip nat translations
Pro  Inside global     Inside local       Outside local      Outside global
icmp 209.165.200.226:14192.168.1.2:14     209.165.200.1:14   209.165.200.1:14
icmp 209.165.200.226:15192.168.1.2:15     209.165.200.1:15   209.165.200.1:15
icmp 209.165.200.226:16192.168.1.2:16     209.165.200.1:16   209.165.200.1:16
icmp 209.165.200.226:17192.168.1.2:17     209.165.200.1:17   209.165.200.1:17
```
#### Ping одновременно с двух хостов
```
R1#show ip nat translations 
Pro  Inside global     Inside local       Outside local      Outside global
icmp 209.165.200.226:1 192.168.1.3:1      209.165.200.1:1    209.165.200.1:1
icmp 209.165.200.226:22192.168.1.2:22     209.165.200.1:22   209.165.200.1:22
icmp 209.165.200.226:23192.168.1.2:23     209.165.200.1:23   209.165.200.1:23
icmp 209.165.200.226:24192.168.1.2:24     209.165.200.1:24   209.165.200.1:24
icmp 209.165.200.226:25192.168.1.2:25     209.165.200.1:25   209.165.200.1:25
icmp 209.165.200.226:26192.168.1.2:26     209.165.200.1:26   209.165.200.1:26
icmp 209.165.200.226:27192.168.1.2:27     209.165.200.1:27   209.165.200.1:27
icmp 209.165.200.226:2 192.168.1.3:2      209.165.200.1:2    209.165.200.1:2
icmp 209.165.200.226:3 192.168.1.3:3      209.165.200.1:3    209.165.200.1:3
icmp 209.165.200.226:4 192.168.1.3:4      209.165.200.1:4    209.165.200.1:4
```
### Настройка PAT на интерфейсе
```
R1(config)#no ip nat inside source list 1 pool PUBLIC_ACCESS overload 
R1(config)#no ip nat pool PUBLIC_ACCESS
R1(config)#ip nat inside source list 1 interface g0/0/0 overload  
```
#### Ping с PC-B
```
R1#show ip nat translations 
Pro  Inside global     Inside local       Outside local      Outside global
icmp 209.165.200.230:5 192.168.1.3:5      209.165.200.1:5    209.165.200.1:5
icmp 209.165.200.230:6 192.168.1.3:6      209.165.200.1:6    209.165.200.1:6
icmp 209.165.200.230:7 192.168.1.3:7      209.165.200.1:7    209.165.200.1:7
icmp 209.165.200.230:8 192.168.1.3:8      209.165.200.1:8    209.165.200.1:8
```
#### Ping с S1 и S2 
```
S1#ping 209.165.200.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 209.165.200.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/2 ms
```
```
S2#ping 209.165.200.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 209.165.200.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
```
```
R1#sh ip nat translations 
Pro  Inside global     Inside local       Outside local      Outside global
icmp 209.165.200.230:1024192.168.1.11:2     209.165.200.1:2    209.165.200.1:1024
icmp 209.165.200.230:1025192.168.1.11:3     209.165.200.1:3    209.165.200.1:1025
icmp 209.165.200.230:1026192.168.1.11:4     209.165.200.1:4    209.165.200.1:1026
icmp 209.165.200.230:1027192.168.1.11:5     209.165.200.1:5    209.165.200.1:1027
icmp 209.165.200.230:13192.168.1.3:13     209.165.200.1:13   209.165.200.1:13
icmp 209.165.200.230:14192.168.1.3:14     209.165.200.1:14   209.165.200.1:14
icmp 209.165.200.230:15192.168.1.3:15     209.165.200.1:15   209.165.200.1:15
icmp 209.165.200.230:16192.168.1.3:16     209.165.200.1:16   209.165.200.1:16
icmp 209.165.200.230:2 192.168.1.12:2     209.165.200.1:2    209.165.200.1:2
icmp 209.165.200.230:32192.168.1.2:32     209.165.200.1:32   209.165.200.1:32
icmp 209.165.200.230:33192.168.1.2:33     209.165.200.1:33   209.165.200.1:33
icmp 209.165.200.230:34192.168.1.2:34     209.165.200.1:34   209.165.200.1:34
icmp 209.165.200.230:35192.168.1.2:35     209.165.200.1:35   209.165.200.1:35
icmp 209.165.200.230:36192.168.1.2:36     209.165.200.1:36   209.165.200.1:36
icmp 209.165.200.230:37192.168.1.2:37     209.165.200.1:37   209.165.200.1:37
icmp 209.165.200.230:38192.168.1.2:38     209.165.200.1:38   209.165.200.1:38
icmp 209.165.200.230:39192.168.1.2:39     209.165.200.1:39   209.165.200.1:39
icmp 209.165.200.230:3 192.168.1.12:3     209.165.200.1:3    209.165.200.1:3
icmp 209.165.200.230:4 192.168.1.12:4     209.165.200.1:4    209.165.200.1:4
icmp 209.165.200.230:5 192.168.1.12:5     209.165.200.1:5    209.165.200.1:5
```
## Часть 4. Настройка и проверка статического NAT для IPv4
```
R1#clear ip nat translations * 
R1#clear ip nat statistics
R1#conf ter
R1(config)#ip nat inside source static 192.168.1.2 209.165.200.229
```
### Проверка
```
R1#show ip nat translations
Pro  Inside global     Inside local       Outside local      Outside global
---  209.165.200.229   192.168.1.2        ---                ---
```
```
R2#ping 209.165.200.229

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 209.165.200.229, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
```
```
R1#show ip nat translations
Pro  Inside global     Inside local       Outside local      Outside global
---  209.165.200.229   192.168.1.2        ---                ---

R1#show ip nat translations
Pro  Inside global     Inside local       Outside local      Outside global
icmp 209.165.200.229:10192.168.1.2:10     209.165.200.225:10 209.165.200.225:10
icmp 209.165.200.229:2 192.168.1.2:2      209.165.200.225:2  209.165.200.225:2
icmp 209.165.200.229:3 192.168.1.2:3      209.165.200.225:3  209.165.200.225:3
icmp 209.165.200.229:4 192.168.1.2:4      209.165.200.225:4  209.165.200.225:4
icmp 209.165.200.229:5 192.168.1.2:5      209.165.200.225:5  209.165.200.225:5
icmp 209.165.200.229:6 192.168.1.2:6      209.165.200.225:6  209.165.200.225:6
icmp 209.165.200.229:7 192.168.1.2:7      209.165.200.225:7  209.165.200.225:7
icmp 209.165.200.229:8 192.168.1.2:8      209.165.200.225:8  209.165.200.225:8
icmp 209.165.200.229:9 192.168.1.2:9      209.165.200.225:9  209.165.200.225:9
---  209.165.200.229   192.168.1.2        ---                ---
```
