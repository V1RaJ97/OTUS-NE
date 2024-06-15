# Лабораторная работа - Настройка протоколов CDP, LLDP и NTP
## Топология
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/0fee2788781863ae5fc4ee2b8eb97cb1f19308f4/Labs/Lab13/%D0%A2%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F.png)
## Таблица адресации

| Устройство |     Интерфейс    |    IP-адрес    | Маска подсети | Шлюз по умолчанию |
|:----------:|:----------------:|:--------------:|:-------------:|:-----------------:|
|     R1     |     Loopback1    |   172.16.1.1   | 255.255.255.0 |                   |
|     R1     |      G0/0/1      |   10.22.0.1    | 255.255.255.0 |                   |
|     S1     |    SVI VLAN 1    |   10.22.0.2    | 255.255.255.0 |     10.22.0.1     | 
|     S2     |    SVI VLAN 1    |   10.22.0.3    | 255.255.255.0 |     10.22.0.1     |

## Задачи
1. Создание сети и настройка основных параметров устройства
2. Обнаружение сетевых ресурсов с помощью протокола CDP
3. Обнаружение сетевых ресурсов с помощью протокола LLDP
4. Настройка и проверка NTP

## Часть 1. Создание сети и настройка основных параметров устройства
### Конфигурация R1
```
R1#sh running-config 
Building configuration...

Current configuration : 936 bytes
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
interface Loopback1
 ip address 172.16.1.1 255.255.255.0
!
interface GigabitEthernet0/0/0
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface GigabitEthernet0/0/1
 ip address 10.22.0.1 255.255.255.0
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

### Конфигурация S1/S2
```
S1#show running-config 
Building configuration...

Current configuration : 1540 bytes
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
 shutdown
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

## Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP
```
R1#show cdp 
Global CDP information:
    Sending CDP packets every 60 seconds
    Sending a holdtime value of 180 seconds
    Sending CDPv2 advertisements is enabled
R1#show cdp neighbors 
Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
S1           Gig 0/0/1        172            S       2960        Fas 0/5

Вопрос: Сколько интерфейсов участвует в объявлениях CDP? Какие из них активны?
Ответ: Один -  Gig 0/0/1
```
```
R1#show cdp entry  S1

Device ID: S1
Entry address(es): 
Platform: cisco 2960, Capabilities: Switch
Interface: GigabitEthernet0/0/1, Port ID (outgoing port): FastEthernet0/5
Holdtime: 166

Version :
Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2013 by Cisco Systems, Inc.
Compiled Wed 26-Jun-13 02:49 by mnguyen

advertisement version: 2
Duplex: full

Вопрос: Какая версия IOS используется на S1?
Ответ: Version 15.0(2)SE4
```

```
S1#show cdp traffic
            ^
% Invalid input detected at '^' marker.
```
### Настройка SVI для VLAN 1 на S1 и S2
```
S1(config)#int vlan1
S1(config-if)#ip address 10.22.0.2 255.255.255.0
S1(config-if)#no shutdown 
S1(config-if)#exit
S1(config)#ip default-gateway 10.22.0.1
S1(config)#exit
```
```
S2(config)#int vlan1
S2(config-if)#ip address 10.22.0.3 255.255.255.0
S2(config-if)#no shutdown 
S2(config-if)#exit
S2(config)#ip default-gateway 10.22.0.1
S2(config)#exit
```
