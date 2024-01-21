# Лабораторная работа. Просмотр таблицы MAC-адресов коммутатора 

## Схема сети
![Alt text](https://github.com/V1RaJ97/OTUS-NE/blob/73e2611f0215698d2d6e5eda8548519dc4197680/Labs/Lab02/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0%20%D1%81%D0%B5%D1%82%D0%B8.png)
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

## Часть 1
### Коммутатор S1
#### Первоначальная настройка
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
#### Ограничичение доступа через консольный порт
```
S1(config)#line console 0
S1(config-line)#logging synchronous 
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
```
#### Настройка VTY
```
S1(config)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
```
#### Настройка Startup-config
```
S1>en
Password: 
S1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
```
#### Настройка Vlan 1
```
S1(config)#interface vlan 1
S1(config-if)#ip address 192.168.1.11 255.255.255.0
S1(config-if)#no shutdown
S1(config-if)#
%LINK-5-CHANGED: Interface Vlan1, changed state to up
S1(config-if)#end
```
#### Финальная конфигурация S1
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
Проделываем все аналогичным образом,
задаем hostname, ip адрес и маску подсети исходя из таблицы адресации.
```
#### Финальная конфигурация S2
```
S2#show running-config 
Building configuration...

Current configuration : 1293 bytes
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
 ip address 192.168.1.12 255.255.255.0
!
banner motd ^C Unauthorized access is strictly prohibited ^C
!
!
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
!
!
!
end

```
### Подключение компьютеров
```
Подключаем компьютеры PC-A к S1, PC-B к S2. Исходя из схемы сети.
Настраиваем на компьютерах ip-адреса и маски подсети исходя из таблицы адресации.
```
## Часть 2
### Таблица MAC-адресов устройств
| Устройство     |       MAC-адрес      |
|:--------------:|:--------------------:|
|        S1      |    000a.4161.6a01    | 
|        S2      |    0003.e4b9.b5e0    | 
|       PC-A     |    0030.A355.A344    | 
|       PC-B     |    000D.BD1C.8E0A    |
### Таблица MAC-адресов S2
```
S2#show mac address-table 
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

   1    000a.4161.6a01    DYNAMIC     Fa0/1
   1    000d.bd1c.8e0a    DYNAMIC     Fa0/18
```
```
После очистки таблицы MAC-адресов на коммутаторе S2 она стала пустой.
Когда прошло 10 секунд появился только MAC-адрес коммутатора S1.
После пинга коммутатора S2 с компьютера PC-B появился и адрес компьютера.
```
### Протокол ARP
```
Выполняя команду "arp -a", на компьютере PC-B получаем следующую информацию:
C:\>arp -a
  Internet Address      Physical Address      Type
  192.168.1.1           0030.a355.a344        dynamic
  192.168.1.12          0060.7014.202c        dynamic
```
### Эхо-запросы с PC-B до остальных устройств
#### Ping с PC-B до S1
![Alt text](https://github.com/V1RaJ97/OTUS-NE/blob/4711197dae28c3555e552a5e1ca392699a80086b/Labs/Lab02/PingS1.png)
#### Ping с PC-B до S2
![Alt text](https://github.com/V1RaJ97/OTUS-NE/blob/9949f53000013946278e9aca368da83254209ceb/Labs/Lab02/PingS2.png)
#### Ping с PC-B до PC-A
![Alt text](https://github.com/V1RaJ97/OTUS-NE/blob/9949f53000013946278e9aca368da83254209ceb/Labs/Lab02/PingPC-A.png)
#### Таблица MAC-адресов S2 после выполнения эхо запросов
```
S2#show mac address-table 
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

   1    000a.4161.6a01    DYNAMIC     Fa0/1
   1    000d.bd1c.8e0a    DYNAMIC     Fa0/18
```
#### ARP-кэш после отправки эхо-запросов
```
C:\>arp -a
  Internet Address      Physical Address      Type
  192.168.1.1           0030.a355.a344        dynamic
  192.168.1.11          0090.2b62.0be3        dynamic
  192.168.1.12          0060.7014.202c        dynamic
```
