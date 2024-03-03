# Лабораторная работа - Внедрение маршрутизации между виртуальными локальными сетями 
## Схема сети
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/45acf54fe52587d6691cee1c7e9ec4668462c79f/Labs/Lab06/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0%20%D1%81%D0%B5%D1%82%D0%B8.png)

## Таблица адресации
| Устройство |  Интерфейс  |   IP-адрес   | Маска подсети | Шлюз по умолчанию |
|:----------:|:-----------:|:------------:|:-------------:|:-----------------:|
|     R1     |  G0/0/1.10  | 192.168.10.1 | 255.255.255.0 | _________________ |
|     R1     |  G0/0/1.20  | 192.168.20.1 | 255.255.255.0 | _________________ |
|     R1     |  G0/0/1.30  | 192.168.30.1 | 255.255.255.0 | _________________ |
|     R1     | G0/0/1.1000 | ____________ | _____________ | _________________ |
|     S1     |    VLAN 10  | 192.168.10.11| 255.255.255.0 |   192.168.10.1    |
|     S2     |    VLAN 10  | 192.168.10.12| 255.255.255.0 |   192.168.10.1    |
|    PC-A    |     NIC     | 192.168.20.3 | 255.255.255.0 |   192.168.20.1    |
|    PC-A    |     NIC     | 192.168.30.3 | 255.255.255.0 |   192.168.30.1    |

## Таблица VLAN
|    VLAN    |     Имя     |    Назначенный интерфейс    |
|:----------:|:-----------:|:---------------------------:|
|     10     | Управление  |       S1,S2: VLAN 10        |
|     20     |    Sales    |          S1: F0/6           |
|     30     |  Operations |          S2: F0/18          |
|    999     | Parking_Lot | С1: F0/2-4, F0/7-24, G0/1-2 |
|    999     | Parking_Lot |С2: F0/2-17, F0/19-24, G0/1-2|
|   1000     | Собственная |_____________________________|
## Задачи
1. Создание сети и настройка основных параметров устройства
2. Создание сетей VLAN и назначение портов коммутатора
3. Настройка транка 802.1Q между коммутаторами.
4. Настройка маршрутизации между сетями VLAN
5. Проверка, что маршрутизация между VLAN работает

## Часть 1. Создание сети и настройка основных параметров устройства
### Настройка базовых параметров маршрутизатора R1
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

R1#clock set 20:38:00 3 march 2024
R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
R1#show clock detail 
20:38:24.114 UTC Sun Mar 3 2024
Time source is user configuration
```
