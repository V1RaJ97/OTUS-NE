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
### Реализация различных оптимизаций на каждом маршрутизаторе
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
