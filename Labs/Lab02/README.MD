# Лабораторная работа. Просмотр таблицы MAC-адресов коммутатора 

## Схема сети
## Таблица адресации
| Устройство     | Интерфейс    |       IP-адрес       | Маска подсети |
|:--------------:|:------------:|:--------------------:|:-------------:|
|        S1      |     VLAN 1   |     192.168.1.11     | 255.255.255.0 |
|        S2      |     VLAN 1   |     192.168.1.12     | 255.255.255.0 |
|       PC-A     |      NIC     |     192.168.1.1      | 255.255.255.0 |
|       PC-B     |      NIC     |     192.168.1.2      | 255.255.255.0 |
## Задание
1. Создание и настройка сети
2. Изучение таблицы МАС-адресов коммутатора

## Настройка коммутаторов
### Коммутатор S1
```
Switch>en
Switch#conf ter
Switch(config)#hostname S1
S1(config)#service password-encryption 
S1(config)#enable secret class
S1(config)#banner motd 1 Unauthorized access is strictly prohibited 1
S1(config)#no ip domain-lookup
S1(config)#end
```
```
S1(config)#line console 0
S1(config-line)#logging synchronous 
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
```
```
S1(config)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
```
```
S1>en
Password: 
S1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
```
```
S1(config)#interface vlan 1
S1(config-if)#ip address 192.168.1.11 255.255.255.0
S1(config-if)#no shutdown
S1(config-if)#
%LINK-5-CHANGED: Interface Vlan1, changed state to up
S1(config-if)#end
```
#### Финальная конфигурация
```
S1#show running-config 
Building configuration...

Current configuration : 1293 bytes
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
!
!
no ip domain-lookup
!
!
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
 ip address 192.168.1.11 255.255.255.0
banner motd ^C Unauthorized access is strictly prohibited ^C

line con 0
 password 7 0822455D0A16
 logging synchronous
 login
line vty 0 4
 password 7 0822455D0A16
 login
line vty 5 15
 login
end
```
### Коммутатор S2
```
Проделываем все аналогичным образом.
Затем задаем ip адрес и маску подсити исходя из таблици адресации.
