# Лабораторная работа - Реализация DHCPv4 
## Топология
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/50260140e22db30f62ca76cf9e5e8543e1c08e15/Labs/Lab08/%D0%A2%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F(ipv4).png)
## Таблица адресации
|  Устройство  |  Интерфейс  |  IP-адрес  |  Маска подсети  | Шлюз по умолчанию |
|:------------:|:-----------:|:----------:|:---------------:|:-----------------:|
|      R1      |    G0/0/0   |  10.0.0.1  | 255.255.255.252 | _________________ |
|      R1      |    G0/0/1   | 192.168.1.0| 255.255.255.198 |                   |
|      R1      | G0/0/1.100  | 192.168.1.1| 255.255.255.198 |                   |
|      R1      | G0/0/1.200  |192.168.1.58| 255.255.255.228 |                   |
|      R1      | G0/0/1.1000 |____________|_________________|                   |
|      R2      |    G0/0     |  10.0.0.2  | 255.255.255.252 | _________________ |
|      R2      |    G0/0/1   |192.168.1.86| 255.255.255.244 |                   |
|      S1      |   VLAN 200  |192.168.1.59| 255.255.255.228 |   192.168.1.58    |
|      S2      |    VLAN 1   |            |                 |                   |
|     PC-A     |     NIC     |    DHCP    |       DHCP      |       DHCP        |
|     PC-B     |     NIC     |    DHCP    |       DHCP      |       DHCP        |
## Таблица VLAN
|   VLAN   |     Имя     |    Назначенный интерфейс    |
|:--------:|:-----------:|:---------------------------:|
|     1    |     Нет     |         S2: F0/18           |
|    100   |   Clients   |          S1: F0/6           |
|    200   |  Management |         S1: VLAN 200        |
|    999   | Parking_Lot | S1: F0/1-4, F0/7-24, G0/1-2 |
|   1000   |   Private   |____________________________ |
## Задачи
1. Создание сети и настройка основных параметров устройства
2. Настройка и проверка двух серверов DHCPv4 на R1
3. Настройка и проверка DHCP-ретрансляции на R2

## Часть 1.	Создание сети и настройка основных параметров устройства
### Базовая настройка маршрутизаторов R1 и R2
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
R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
Проводим аналогичные настройки на R2
