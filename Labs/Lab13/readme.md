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
```
