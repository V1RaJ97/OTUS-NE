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
### Настройка коммутаоров
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
```
