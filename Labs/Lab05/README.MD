# Лабораторная работа. Доступ к сетевым устройствам по протоколу SSH
## Схема сети
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/fbbe157e880ad15dde4f484c994a66909a232098/Labs/Lab05/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0%20%D1%81%D0%B5%D1%82%D0%B8.png)
## Таблица адресации
| Устройство | Интерфейс |   IP-адрес  | Маска подсети | Шлюз по умолчанию |
|:----------:|:---------:|:-----------:|:-------------:|:-----------------:|
|     R1     |   G0/0/1  | 192.168.1.1 | 255.255.255.0 | _________________ |
|     S1     |   VLAN 1  | 192.168.1.11| 255.255.255.0 |    192.168.1.1    |
|    PC-A    |    NIC    | 192.168.1.3 | 255.255.255.0 |    192.168.1.1    |

## Задачи
1. Настройка основных параметров устройства
2. Настройка маршрутизатора для доступа по протоколу SSH
3. Настройка коммутатора для доступа по протоколу SSH
4. SSH через интерфейс командной строки (CLI) коммутатора
## Часть 1. Настройка основных параметров устройств
### Конфигурация коммутатора
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
no ip domain-lookup
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
interface FastEthernet0/2
interface FastEthernet0/3
interface FastEthernet0/4
interface FastEthernet0/5
interface FastEthernet0/6
interface FastEthernet0/7
interface FastEthernet0/8
interface FastEthernet0/9
interface FastEthernet0/10
interface FastEthernet0/11
interface FastEthernet0/12
interface FastEthernet0/13
interface FastEthernet0/14
interface FastEthernet0/15
interface FastEthernet0/16
interface FastEthernet0/17
interface FastEthernet0/18
interface FastEthernet0/19
interface FastEthernet0/20
interface FastEthernet0/21
interface FastEthernet0/22
interface FastEthernet0/23
interface FastEthernet0/24
interface GigabitEthernet0/1
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
### Настройка маршрутизатора
```
Router(config)#hostname R1
R1(config)#no ip domain-lookup
R1(config)#enable secret class
R1(config)#service password-encryption
R1(config)#banner motd 1 Unauthorized access is strictly prohibited 1

R1(config)#line console 0
R1(config-line)#logging synchronous 
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#end

R1(config)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#end
```
