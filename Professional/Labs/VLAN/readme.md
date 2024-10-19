# Лабораторная работа - настройка маршрутизации между VLAN-сетями Router-on-a-Stick
## Схема сети
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/6001e275ef472672e0e3d10665f01b51e62f0bb7/Professional/Labs/VLAN/vlan.png)
## Таблица адресации
| Устройство |  Интерфейс  |   IP-адрес   | Маска подсети | Шлюз по умолчанию |
|:----------:|:-----------:|:------------:|:-------------:|:-----------------:|
|     R1     |   G0/0/1.3  |  192.168.3.1 | 255.255.255.0 | _________________ |
|     R1     |   G0/0/1.4  |  192.168.4.1 | 255.255.255.0 | _________________ |
|     R1     |   G0/0/1.8  | ____________ | _____________ | _________________ |
|     S1     |    VLAN 3   | 192.168.3.11 | 255.255.255.0 |    192.168.3.1    |
|     S2     |    VLAN 3   | 192.168.3.12 | 255.255.255.0 |    192.168.3.1    |
|    PC-A    |     NIC     | 192.168.3.3  | 255.255.255.0 |    192.168.3.1    |
|    PC-A    |     NIC     | 192.168.4.3  | 255.255.255.0 |    192.168.4.1    |

## Таблица VLAN
|    VLAN    |     Имя     |    Назначенный интерфейс    |
|:----------:|:-----------:|:---------------------------:|
|     3      | Management  |S1:VLAN 3; S2:VLAN 3; S1:F0/6|
|     4      |  Operations |          S2: F0/18          |
|     7      | Parking_Lot | S1: F0/2-4, F0/7-24, G0/1-2 |
|     7      | Parking_Lot |S2: F0/2-17, F0/19-24, G0/1-2|
|     8      |   Native    |_____________________________|
## Задачи
1. Создание сети и настройка основных параметров устройства
2. Создание сетей VLAN и назначение портов коммутатора
3. Настройка транка 802.1Q между коммутаторами.
4. Настройка маршрутизации между сетями VLAN
5. Проверка, что маршрутизация между VLAN работает

## Цели
1. Создание сети и настройка основных параметров устройства
2. Создание VLAN и назначение портов коммутатора
3. Настройка транка 802.1Q между коммутаторами
4. Настройка маршрутизации между VLAN на маршрутизаторе Часть
5. Проверка работы маршрутизации между VLAN

## Часть 1: Создание сети и настройка основных параметров устройства
### Базовая настройка маршрутизатора R1
```
Router(config)#hostname R1
R1(config)#no ip domain-lookup
R1(config)#enable secret class
R1(config)#service password-encryption 
R1(config)#banner motd 1 Unauthorized access is strictly prohibited 1
```
```
R1(config)#line console 0
R1(config-line)#logging synchronous 
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
```
```
R1(config)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#end
```
```
R1#clock set 16:15:00 19 october 2024
R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
### Конфигурация R1
```
R1#show running-config 
Building configuration...

Current configuration : 760 bytes
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
 login
!
end
```
### Настройка базовых параметров коммутаторов S1 и S2
```
S1(config)#hostname S1
S1(config)#service password-encryption 
S1(config)#enable secret class
S1(config)#banner motd 1 Unauthorized access is strictly prohibited 1
S1(config)#no ip domain-lookup
```
```
S1(config)#line console 0
S1(config-line)#logging synchronous 
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
```
```
S1(config)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
```
```
S1#clock set 16:20:00 19 october 2024
S1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
```
Производим аналогичную настройку на S2
```
### Конфигурация S1/S2
```
S2#show running-config 
Building configuration...

Current configuration : 1279 bytes
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
 login
line vty 5 15
 login
!
end
```
