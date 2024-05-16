# Лабораторная работа. Настройка и проверка расширенных списков контроля доступа.
## Топология
![alt-text](https://github.com/V1RaJ97/OTUS-NE/blob/34e6246803fccf4daef8983c857639c70f5c92d6/Labs/Lab11/%D0%A2%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F.png)
## Таблица адресации
| Устройство |     Интерфейс    |    IP-адрес    | Маска подсети | Шлюз по умолчанию |
|:----------:|:----------------:|:--------------:|:-------------:|:-----------------:|
|     R1     |      G0/0/1      |   _________    | _____________ |                   |
|     R1     |     G0/0/1.20    |   10.20.0.1    | 255.255.255.0 |                   |
|     R1     |     G0/0/1.30    |   10.30.0.1    | 255.255.255.0 |                   | 
|     R1     |     G0/0/1.40    |   10.40.0.1    | 255.255.255.0 |                   |
|     R1     |    G0/0/1.1000   |   _________    | _____________ |                   |
|     R1     |     Loopback1    |   172.16.1.1   | 255.255.255.0 |                   |
|     R2     |      G0/0/1      |   10.20.0.4    | 255.255.255.0 |                   | 
|     S1     |      VLAN 20     |   10.20.0.2    | 255.255.255.0 |     10.20.0.1     |
|     S2     |      VLAN 20     |   10.20.0.3    | 255.255.255.0 |     10.20.0.1     |
|    PC-A    |        NIC       |   10.30.0.10   | 255.255.255.0 |     10.30.0.1     |
|    PC-B    |        NIC       |   10.40.0.10   | 255.255.255.0 |     10.40.0.1     |

## Таблица VLAN
| VLAN |     Имя    |         Назначенный интерфейс         |
|:----:|:----------:|:-------------------------------------:|
|  20  | Management |               S2: F0/5                |
|  30  | Operations |               S1: F0/6                |
|  40  |   Sales    |               S2: F0/18               |
| 999  | ParkingLot |      S1: F0/2-4, F0/7-24, G0/1-2      |
| 999  | ParkingLot | S2: F0/2-4, F0/6-17, F0/19-24, G0/1-2 |
| 1000 |  Private   |   __________________________________  |
## Задачи
1. Создание сети и настройка основных параметров устройства
2. Настройка и проверка списков расширенного контроля доступа

## Часть 1. Создание сети и настройка основных параметров устройства
### Финальная конфигурация маршрутизаторов на примере R2
```
R2#show running-config 
Building configuration...

Current configuration : 866 bytes
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
end
```
### Финальная конфигурация коммутаторов на примере S1
```
S1#show running-config 
Building configuration...

Current configuration : 1300 bytes
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
end
```
## Часть 2. Настройка сетей VLAN на коммутаторах.
### Создание сети VLAN на коммутаторах
```
S1(config)#vlan 20
S1(config-vlan)#name Management
S1(config-vlan)#exit
S1(config)#vlan 30
S1(config-vlan)#name Operations
S1(config-vlan)#exit
S1(config)#vlan 40
S1(config-vlan)#name Sales
S1(config-vlan)#exit
S1(config)#vlan 999
S1(config-vlan)#name ParkingLot
S1(config-vlan)#exit
S1(config)#vlan 1000
S1(config-vlan)#name Private
S1(config-vlan)#exit
S1(config)#ip de
S1(config)#ip default-gateway 10.20.0.1
!
Проделываем аналогичные действия на S2
```
#### Настройка VLAN на S2
```
S2(config)#int vlan 20
S2(config-if)#ip address 10.20.0.3 255.255.255.0
S2(config-if)#end
S2(config)#int f0/5
S2(config-if)#switchport mode access 
S2(config-if)#switchport access vlan 20
S2(config-if)#shutdown
S2(config-if)#no shutdown 
S2(config-if)#exit
S2(config)#int f0/18
S2(config-if)#switchport mode access 
S2(config-if)#switchport access vlan 40
S2(config-if)#shutdown 
S2(config-if)#no shutdown 
S2(config-if)#end
```
```
S2(config)#interface range f0/2-4
S2(config-if-range)#switchport mode access 
S2(config-if-range)#switchport access vlan 999
S2(config-if-range)#shutdown 
S2(config-if-range)#exit
S2(config)#int range f0/6-17
S2(config-if-range)#switchport mode access 
S2(config-if-range)#switchport access vlan 999
S2(config-if-range)#shutdown 
S2(config-if-range)#exit
S2(config)#int range f0/19-24
S2(config-if-range)#switchport mode access 
S2(config-if-range)#switchport access vlan 999
S2(config-if-range)#shutdown 
S2(config-if-range)#exit
S2(config)#int range g0/1-2
S2(config-if-range)#switchport mode access 
S2(config-if-range)#switchport access vlan 999
S2(config-if-range)#shutdown 
S2(config-if-range)#exit
```

#### Настройка VLAN на S1
```
S1(config)#int f0/6
S1(config-if)#switchport access vlan 30
S1(config-if)#shutdown
S1(config-if)#no shutdown 
S1(config-if)#exit
S1(config)#int range f0/2-4
S1(config-if-range)#switchport mode access 
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#shutdown 
S1(config-if-range)#exit
S1(config)#int range f0/7-24
S1(config-if-range)#switchport mod access 
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#shutdown 
S1(config-if-range)#exit
S1(config)#int range g0/1-2
S1(config-if-range)#switchport mode access 
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#shutdown 
S1(config-if-range)#exit
```
#### Результат
```
S1#show vlan brief 

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/5
20   Management                       active    
30   Operations                       active    Fa0/6
40   Sales                            active    
999  ParkingLot                       active    Fa0/2, Fa0/3, Fa0/4, Fa0/7
                                                Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gig0/1, Gig0/2
1000 Private                          active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active  
```
```
S2#show vlan brief 

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1
20   Management                       active    Fa0/5
30   Operations                       active    
40   Sales                            active    Fa0/18
999  ParkingLot                       active    Fa0/2, Fa0/3, Fa0/4, Fa0/6
                                                Fa0/7, Fa0/8, Fa0/9, Fa0/10
                                                Fa0/11, Fa0/12, Fa0/13, Fa0/14
                                                Fa0/15, Fa0/16, Fa0/17, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gig0/1, Gig0/2
1000 Private                          active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active   
```
## Часть 3. ·Настройка магистральных каналов.
### Настройка магистрального интерфейса F0/1
```
S1(config)#int f0/1
S1(config-if)#switchport mode trunk
S1(config-if)#switchport trunk native vlan 1000
S1(config-if)#switchport trunk allowed vlan 20,30,40,1000
S1(config-if)#shutdown
S1(config-if)#no shutdown 
S1(config-if)#exit
!
S2(config)#int f0/1
S2(config-if)#switchport mode trunk
S2(config-if)#switchport trunk native vlan 1000
S2(config-if)#switchport trunk allowed vlan 20,30,40,1000
S2(config-if)#shutdown 
S2(config-if)#no shutdown 
S2(config-if)#exit
```
#### Результаты
```
S1#show interfaces trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      1000

Port        Vlans allowed on trunk
Fa0/1       20,30,40,1000

Port        Vlans allowed and active in management domain
Fa0/1       20,30,40,1000

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       20,30,40,1000

```
```
S2#show interfaces trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      1000

Port        Vlans allowed on trunk
Fa0/1       20,30,40,1000

Port        Vlans allowed and active in management domain
Fa0/1       20,30,40,1000

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       20,30,40,1000
```
### Настройка магистрального интерфейса F0/5 на коммутаторе S1.
```
S1(config)#int f0/5
S1(config-if)#switchport mode trunk 
S1(config-if)#switchport trunk native vlan 1000
S1(config-if)#switchport trunk allowed vlan 20,30,40,1000
S1(config-if)#shutdown 
S1(config-if)#no shutdown 
S1(config-if)#end
S1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
## Настройка маршрутизации.
### Настройка маршрутизации между сетями VLAN на R1
```
R1(config)#int g0/0/1
R1(config-if)#no shutdown
R1(config-if)#exit
R1(config)#int g0/0/1.20
R1(config-subif)#encapsulation dot1Q 20
R1(config-subif)#description Management
R1(config-subif)#ip address 10.20.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.30
R1(config-subif)#encapsulation dot1Q 30
R1(config-subif)#description Operations
R1(config-subif)#ip address 10.30.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.40
R1(config-subif)#encapsulation dot1Q 40
R1(config-subif)#description Sales
R1(config-subif)#ip address 10.40.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.1000
R1(config-subif)#encapsulation dot1Q 1000
R1(config-subif)#description Private
R1(config-subif)#exit
```
```
R1(config)#int loopback1
R1(config-if)#ip address 172.16.1.1 255.255.255.0
R1(config-if)#exit
```
```
R1#show ip interface brief 
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES unset  administratively down down 
GigabitEthernet0/0/1   unassigned      YES unset  up                    up 
GigabitEthernet0/0/1.2010.20.0.1       YES manual up                    up 
GigabitEthernet0/0/1.3010.30.0.1       YES manual up                    up 
GigabitEthernet0/0/1.4010.40.0.1       YES manual up                    up 
GigabitEthernet0/0/1.1000unassigned      YES unset  up                    up 
GigabitEthernet0/0/2   unassigned      YES unset  administratively down down 
Loopback1              172.16.1.1      YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
```
### Настройка интерфейса R2 g0/0/1 с использованием адреса из таблицы и маршрута по умолчанию с адресом следующего перехода 10.20.0.1
```
R2(config)#int g0/0/1
R2(config-if)#no shutdown 
R2(config-if)#ip address 10.20.0.4 255.255.255.0
R2(config-if)#exit
R2(config)#ip route 0.0.0.0 0.0.0.0 10.20.0.1
```
## Настройка удаленного доступа
### Настройка сетевых устройств для базовой поддержки SSH
```
R1(config)#username SSHadmin secret $cisco123!
R1(config)#ip domain name ccna-lab.com
R1(config)#crypto key generate rsa
The name for the keys will be: R1.ccna-lab.com
How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
R1(config)#ip ssh version 2
R1(config)#line vty 0 4
R1(config-line)#login local
R1(config-line)#transport input ssh
R1(config-line)#end

Повторяем те же действия на R2, S1 и S2
```

### Активация защищенных веб-служб с проверкой подлинности на R1
```
R1(config)#ip http secure-server 
               ^
% Invalid input detected at '^' marker.
	
R1(config)#ip http authentication local
               ^
% Invalid input detected at '^' marker.
```
## Часть 6. Проверка подключения
### PC-A
```
C:\>ping 10.40.0.10

Pinging 10.40.0.10 with 32 bytes of data:

Reply from 10.40.0.10: bytes=32 time<1ms TTL=127
Reply from 10.40.0.10: bytes=32 time<1ms TTL=127
Reply from 10.40.0.10: bytes=32 time<1ms TTL=127
Reply from 10.40.0.10: bytes=32 time<1ms TTL=127

Ping statistics for 10.40.0.10:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
```
C:\>ping 10.20.0.1

Pinging 10.20.0.1 with 32 bytes of data:

Reply from 10.20.0.1: bytes=32 time<1ms TTL=255
Reply from 10.20.0.1: bytes=32 time<1ms TTL=255
Reply from 10.20.0.1: bytes=32 time<1ms TTL=255
Reply from 10.20.0.1: bytes=32 time<1ms TTL=255

Ping statistics for 10.20.0.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
### PC-B
```
C:\>ping 10.30.0.10

Pinging 10.30.0.10 with 32 bytes of data:

Reply from 10.30.0.10: bytes=32 time<1ms TTL=127
Reply from 10.30.0.10: bytes=32 time<1ms TTL=127
Reply from 10.30.0.10: bytes=32 time<1ms TTL=127
Reply from 10.30.0.10: bytes=32 time<1ms TTL=127

Ping statistics for 10.30.0.10:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
```
C:\>ping 10.20.0.1

Pinging 10.20.0.1 with 32 bytes of data:

Reply from 10.20.0.1: bytes=32 time<1ms TTL=255
Reply from 10.20.0.1: bytes=32 time<1ms TTL=255
Reply from 10.20.0.1: bytes=32 time<1ms TTL=255
Reply from 10.20.0.1: bytes=32 time<1ms TTL=255

Ping statistics for 10.20.0.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
```
C:\>ping 172.16.1.1

Pinging 172.16.1.1 with 32 bytes of data:

Reply from 172.16.1.1: bytes=32 time<1ms TTL=255
Reply from 172.16.1.1: bytes=32 time<1ms TTL=255
Reply from 172.16.1.1: bytes=32 time<1ms TTL=255
Reply from 172.16.1.1: bytes=32 time<1ms TTL=255

Ping statistics for 172.16.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
```
Сетевой доступ по HTTPS к 10.20.0.1 и 172.16.1.1 отсутствует
Сетевой доступ по SSH к 10.20.0.1 и 172.16.1.1 получен
```
## Часть 7. Настройка и проверка списков контроля доступа (ACL)
