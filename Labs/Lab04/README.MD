# Лабораторная работа. Настройка IPv6-адресов на сетевых устройствах 
## Схема сети
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/c0a304e29ad69d7075d00510eaba3fae2f2d9658/Labs/Lab04/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0%20c%D0%B5%D1%82%D0%B8.png)

## Таблица адресации
|   Устройство   |   Интерфейс  |      IPv6-адрес      | Длинна префикса | Шлюз по умолчанию |
|:--------------:|:------------:|:--------------------:|:---------------:|:-----------------:|
|       R1       |     G0/0/0   |  2001:db8:acad:a::1  |        64       |         __        |
|       R1       |     G0/0/1   |  2001:db8:acad:1::1  |        64       |         __        |
|       S1       |     VLAN 1   |  2001:db8:acad:1::b  |        64       |         __        |
|      PC-A      |      NIC     |  2001:db8:acad:1::3  |        64       |       fe80::1     |
|      PC-B      |      NIC     |  2001:db8:acad:a::3  |        64       |       fe80::1     |

## Задачи
1. Настройка топологии и конфигурация основных параметров маршрутизатора и коммутатора
2. Ручная настройка IPv6-адресов
3. Проверка сквозного соединения

## Часть 1
### Настройка маршрутизатора
#### Первичная настройка
```
Router>en
Router#conf ter
Router(config)#hostname R1
R1(config)#service password-encryption 
R1(config)#enable secret class
R1(config)#banner motd 1 Unauthorized access is strictly prohibited 1
R1(config)#no ip domain-lookup
R1(config)#exit
```
#### Ограничичение доступа через консольный порт
```
R1(config)#line console 0
R1(config-line)#logging synchronous 
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#end
```
#### Настройка VTY
```
R1(config)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#end
```
#### Настройка Startup-config
```
R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
### Настройка коммутатора
#### Первичная настройка
```
Switch>en
Switch#conf ter
Switch(config)#hostname S1
S1(config)#service password-encryption 
S1(config)#enable secret class
S1(config)#banner motd 1 Unauthorized access is strictly prohibited 1
S1(config)#no ip domain-lookup
S1(config)#sdm prefer dual-ipv4-and-ipv6 default
S1(config)#end
S1#reload
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
## Часть2
