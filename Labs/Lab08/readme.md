# Лабраторная работа - Настройка DHCPv6 
## Топология 
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/a9905dd0fc8b8f88ac47328d01d1814df717803c/Labs/Lab08/%D0%A2%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F.png)
## Таблица адресации
|  Устройство  |  Интерфейс  |           IPv6-адрес            |
|:------------:|:-----------:|:-------------------------------:|
|      R1      |    G0/0/0   | 2001:db8:acad:2::1/64 (fe80::1) |
|      R1      |    G0/0/1   | 2001:db8:acad:1::1/64 (fe80::1) |
|      R2      |    G0/0/0   | 2001:db8:acad:2::2/64 (fe80::2) |
|      R2      |    G0/0/1   | 2001:db8:acad:3::1/64 (fe80::1) |
|     PC-A     |     NIC     |              DHCP               |
|     PC-B     |     NIC     |              DHCP               |

## Задачи
1. Создание сети и настройка основных параметров устройства
2. Проверка назначения адреса SLAAC от R1
3. Настройка и проверка сервера DHCPv6 без гражданства на R1
4. Настройка и проверка состояния DHCPv6 сервера на R1
5. Настройка и проверка DHCPv6 Relay на R2

## Часть 1. Создание сети и настройка основных параметров устройства
### Настройка коммутаоров S1 и S2
```
Switch>en
Switch#conf ter
Switch(config)#hostname S1
S1(config)#no ip domain-lookup
S1(config)#service password-encryption 
S1(config)#enable secret class
S1(config)#banner motd 1 Unauthorized access is strictly prohibited 1
```
```
S1(config)#line con 0
S1(config-line)#logging synchronous 
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
```
```
S1(config)#line vty 0 4
S1(config-line)#logging synchronous 
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
```
```
S1(config)#interface range fa0/1-4
S1(config-if-range)#shutdown 
S1(config-if-range)#exit
S1(config)#interface range fa0/7-24
S1(config-if-range)#shutdown 
S1(config-if-range)#exit
S1(config)#interface range gi0/1-2
S1(config-if-range)#shutdown 
S1(config-if-range)#end
S1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```

#### Финальная конфигурация коммутатора S2
```
S2#show running-config 
Building configuration...

Current configuration : 1550 bytes
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
 shutdown
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
### Настройка маршрутизаторов R1 и R2
```
Router(config)#hostname R1
R1(config)#no ip domain-lookup
R1(config)#service password-encryption 
R1(config)#enable secret class
R1(config)#banner motd 1 Unauthorized access is strictly prohibited 1
```
```
R1(config)#line con 0
R1(config-line)#logging synchronous 
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
```
```
R1(config)#line vty 0 4
R1(config-line)#logging synchronous 
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
```
```
R1(config)#ipv6 unicast-routing 
R1(config)#exit
R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
