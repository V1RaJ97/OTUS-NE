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
