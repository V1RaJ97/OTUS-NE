# Лабораторная работа. Настройка протокола OSPFv2 для одной области
## Топология
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/d43be25fb844065bdae833cded00808cf14ed660/Labs/Lab10/%D0%A2%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F.png)
## Таблица адресации
| Устройство |     Интерфейс    |    IP-адрес    | Маска подсети |
|:----------:|:----------------:|:--------------:|:-------------:|
|     R1     |      G0/0/1      |   10.53.0.1    | 255.255.255.0 |
|     R1     |     Loopback1    |   172.16.1.1   | 255.255.255.0 |
|     R2     |      G0/0/1      |   10.53.0.2    | 255.255.255.0 |
|     R2     |     Loopback1    |   192.168.1.1  | 255.255.255.0 |
## Цели
1. Создание сети и настройка основных параметров устройства
2. Настройка и проверка базовой работы протокола  OSPFv2 для одной области
3. Оптимизация и проверка конфигурации OSPFv2 для одной области

## Часть 1. Создание сети и настройка основных параметров устройства
### Конфигурация маршрутизаторов после базовой настройки
```
R1#show running-config
Building configuration...

Current configuration : 864 bytes
!
version 15.4
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
 no ip address
 duplex auto
 speed auto
 shutdown
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
!
end
```

### Конфигурация коммутаторов после базовой настройки
```
S2#show running-config
Building configuration...

Current configuration : 1300 bytes
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
!
interface FastEthernet0/3
!
interface FastEthernet0/4
!
interface FastEthernet0/5
!
interface FastEthernet0/6
!
interface FastEthernet0/7
!
interface FastEthernet0/8
!
interface FastEthernet0/9
!
interface FastEthernet0/10
!
interface FastEthernet0/11
!
interface FastEthernet0/12
!
interface FastEthernet0/13
!
interface FastEthernet0/14
!
interface FastEthernet0/15
!
interface FastEthernet0/16
!
interface FastEthernet0/17
!
interface FastEthernet0/18
!
interface FastEthernet0/19
!
interface FastEthernet0/20
!
interface FastEthernet0/21
!
interface FastEthernet0/22
!
interface FastEthernet0/23
!
interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
 no ip address
 shutdown
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
!
end
```
## Часть 2. Настройка и проверка базовой работы протокола OSPFv2 для одной области
### Настройка адресов на интерфейсах
```
R1(config)#int g0/0/1
R1(config-if)#no shutdown 
R1(config-if)#ip address 10.53.0.1 255.255.255.0
R1(config-if)#exit
R1(config)#int loopback1
R1(config-if)#ip address 172.16.1.1 255.255.255.0
R1(config-if)#no shutdown 
R1(config-if)#exit
```
```
R2(config)#int g0/0/1
R2(config-if)#ip address 10.53.0.2 255.255.255.0
R2(config-if)#no shutdown
R2(config-if)#exit
R2(config)#int loopback1
R2(config-if)#ip address 192.168.1.1 255.255.255.0
R2(config-if)#no shutdown
R2(config-if)#exit
```
### Настройка OSPF на интерфейсах
```
R1(config)#router ospf 56
R1(config-router)#router-id 1.1.1.1
R1(config-router)#exit
R1(config)#int g0/0/1
R1(config-if)#ip ospf 56 area 0
R1(config-if)#exit
```
```
R2(config)#router ospf 56
R2(config-router)#router-id 2.2.2.2
R2(config-router)#exit
R2(config)#int g0/0/1
R2(config-if)#ip ospf 56 area 0
R2(config-if)#exit
R2(config)#int loopback1
R2(config-if)#ip ospf 56 area 0
R2(config-if)#exit
```
```
R1#show ip ospf neighbor 
Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   FULL/BDR        00:00:38    10.53.0.2       GigabitEthernet0/0/1

R2#show ip ospf neighbor 
Neighbor ID     Pri   State           Dead Time   Address         Interface
1.1.1.1           1   FULL/DR         00:00:39    10.53.0.1       GigabitEthernet0/0/1
R2#
```
```
R1#show ip route ospf
     192.168.1.0/32 is subnetted, 1 subnets
O       192.168.1.1 [110/2] via 10.53.0.2, 00:10:00, GigabitEthernet0/0/1
```
```
R1#ping 192.168.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/1 ms
```
## Часть 3. Оптимизация и проверка конфигурации OSPFv2 для одной области
```
R1(config)#int g0/0/1
R1(config-if)#ip ospf priority 50
R1(config-if)#exit
!
R1#show ip ospf interface 
GigabitEthernet0/0/1 is up, line protocol is up
  Internet address is 10.53.0.1/24, Area 0
  Process ID 56, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 1
  Transmit Delay is 1 sec, State DR, Priority 50
  Designated Router (ID) 1.1.1.1, Interface address 10.53.0.1
  Backup Designated Router (ID) 2.2.2.2, Interface address 10.53.0.2
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    Hello due in 00:00:07
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 1, Adjacent neighbor count is 1
    Adjacent with neighbor 2.2.2.2  (Backup Designated Router)
  Suppress hello for 0 neighbor(s)
```
```
R1(config)#interface g0/0/1
R1(config-if)#ip ospf hello-interval 30
R1(config-if)#exit
!
R2(config)#int g0/0/1
R2(config-if)#ip ospf hello-interval 30
R2(config-if)#exit
```
### Настройка и распространение маршрута по умолчанию
```
R1(config)#ip route 0.0.0.0 0.0.0.0 loopback1
%Default route without gateway, if not a point-to-point interface, may impact performance
R1(config)#router ospf 56
R1(config-router)#default-information originate 
R1(config-router)#exit
```
```
R2#show ip route 
Gateway of last resort is 10.53.0.1 to network 0.0.0.0
     10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C       10.53.0.0/24 is directly connected, GigabitEthernet0/0/1
L       10.53.0.2/32 is directly connected, GigabitEthernet0/0/1
     192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.1.0/24 is directly connected, Loopback1
L       192.168.1.1/32 is directly connected, Loopback1
O*E2 0.0.0.0/0 [110/1] via 10.53.0.1, 00:01:43, GigabitEthernet0/0/1
```
### Настройка конфигурации OSPF
```
R1(config)#int gi0/0/1
R1(config-if)#ip ospf network point-to-point 
R1(config-if)#exit
!
R2(config)#int g0/0/1
R2(config-if)#ip ospf network point-to-point 
R2(config-if)#exit
R2(config)#exit
!
R2#show ip ospf neighbor 
Neighbor ID     Pri   State           Dead Time   Address         Interface
1.1.1.1           0   FULL/  -        00:00:28    10.53.0.1       GigabitEthernet0/0/1
```
```
R2(config)#router ospf 56
R2(config-router)#passive-interface loopback1
R2(config-router)#end
!
R2#show ip protocols 
Routing Protocol is "ospf 56"
  Outgoing update filter list for all interfaces is not set 
  Incoming update filter list for all interfaces is not set 
  Router ID 2.2.2.2
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
  Passive Interface(s): 
    Loopback1
  Routing Information Sources:  
    Gateway         Distance      Last Update 
    1.1.1.1              110      00:07:18
    2.2.2.2              110      00:06:17
  Distance: (default is 110)
```
```
R1(config)#router ospf 56
R1(config-router)#auto-cost reference-bandwidth 1000
% OSPF: Reference bandwidth is changed.
R1(config-router)#end
R1#clear ip ospf process 
Reset ALL OSPF processes? [no]: y
R1#
01:19:37: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Adjacency forced to reset

01:19:37: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached

R1#
01:19:39: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done
```
```
R2(config)#router ospf 56
R2(config-router)#auto-cost reference-bandwidth 1000
% OSPF: Reference bandwidth is changed.
        Please ensure reference bandwidth is consistent across all routers.
R2(config-router)#end
R2#clear ip ospf process 
Reset ALL OSPF processes? [no]: y
R2#
01:16:51: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Adjacency forced to reset

01:16:51: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached

R2#
01:17:12: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done
```
### Проверка реализации OSPF
```
R1(config)#int g0/0/1
R1(config-if)#ip ospf dead-interval 120
R1(config-if)#exit
!
R2(config)#int g0/0/1
R2(config-if)#ip ospf dead-interval 120
R2(config-if)#exit
```
#### Show ip ospf interface g0/0/1 на R1
```
GigabitEthernet0/0/1 is up, line protocol is up
  Internet address is 10.53.0.1/24, Area 0
  Process ID 56, Router ID 1.1.1.1, Network Type POINT-TO-POINT, Cost: 10
  Transmit Delay is 1 sec, State POINT-TO-POINT,
  Timer intervals configured, Hello 30, Dead 120, Wait 120, Retransmit 5
    Hello due in 00:00:00
  Index 1/1, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 1 , Adjacent neighbor count is 1
    Adjacent with neighbor 2.2.2.2
  Suppress hello for 0 neighbor(s)
```
#### Show ip route ospf на R1
```
R1#show ip route ospf
     192.168.1.0/32 is subnetted, 1 subnets
O       192.168.1.1 [110/10] via 10.53.0.2, 00:01:52, GigabitEthernet0/0/1
```
#### Show ip route ospf на R2
```
R2#show ip route ospf 
O*E2 0.0.0.0/0 [110/1] via 10.53.0.1, 00:03:54, GigabitEthernet0/0/1
```
#### Резульаты Ping с R2 до интерфейса Loopback1 на R1
```
R2#ping 172.16.1.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/1 ms
```
