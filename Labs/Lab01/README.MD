# 	Лабораторная работа. Базовая настройка коммутатора 
##	Задание
1. Проверка конфигурации коммутатора по умолчанию
2. Создание сети и настройка основных параметров устройства
-	Настройте базовые параметры коммутатора.
-	Настройте IP-адрес для ПК.
3. Проверка сетевых подключений
-	Отобразите конфигурацию устройства.
-	Протестируйте сквозное соединение, отправив эхо-запрос.
-	Протестируйте возможности удаленного управления с помощью Telnet.

## Cхема сети, выполненная в draw.io:
![alt-текст](https://github.com/V1RaJ97/OTUS-NE/blob/da2bf03a3fba533db57866573c0d9842c7244518/Labs/Lab01/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0%20%D1%81%D0%B5%D1%82%D0%B8.png)

## 	Таблица адресации
| Устройство     | Интерфейс    | IP-адрес / префикс       | 
|:--------------:|:------------:|:------------------------:|
|        S1      |     VLAN 1   |     192.168.1.2 /24      |
|       PC-A     |      NIC     |     192.168.1.10 /24     |

## Создание сети и проверка настроек коммутатора по умолчанию
Для первоначальной настройки коммутатора необходимо подключить компьютер PC-A  к коммутатору S1 консольным кабелем и подключиться через терменал. Telnet и SSH возможно использовать только когда настраиваемый коммутатор будет доступен по сети. Сначала нужно присвоить ip адрес на интерфейсе vlan 1 и настроить vty линии. Далее активировать прикладной протокол удаленного доступа и настроить тип аутентификации, в базовом состоянии данные настройки отсутствуют.

## Настройка коммутатора

### Первоначальная настройка Switch:
```
Заходим в привилегированный режим, далее в режим конфигурации.
Задаем имя коммутатора (S1), включаем режим шифрования паролей и задаем пароль для доступа в привилегированный режим (class).
Заполняем текст для motd (Unauthorized access is strictly prohibited).
Отключаем трансляцию имён в ip адреса основанную на dns.
```
```
Switch>en
Switch#conf ter
Switch(config)#hostname S1
S1(config)#service password-encryption
S1(config)#enable secret class
S1(config)#banner motd 1 Unauthorized access is strictly prohibited 1
S1(config)#no ip domain-lookup
```
### Ограничиваем доступ через консольный порт:
```
S1>en
S1#conf ter
S1(config)#line console 0
S1(config-line)#logging synchronous 
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
```
### Конфгурация:
```
S1#show running-config 
Building configuration...
Current configuration : 1292 bytes
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
hostname S1
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
no ip domain-lookup
spanning-tree mode pvst
spanning-tree extend system-id
```
### Настройка VTY:
```
Включаем VTY линии с 0 по 4 и задаем пароль cisco
```
```
S1>en
S1#conf ter
S1(config)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
```
### Конфигурация:
```
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
```
### Настройка Startup-config:
```
С помощью команды "copy running-config startup-config" записываем running-config в startup-config.
```
```
S1>
S1>en
S1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
### Настройка VLAN1:
```
Заходим на интерфейс vlan 1, задаем ip-address и маску подсети.
Изначально интерфейс находится в отключенном состоянии, поднимаем его командой "no shutdown"
```
```
S1#conf ter
S1(config)#interface vlan 1
S1(config-if)#ip address 192.168.1.2 255.255.255.0
S1(config-if)#no shutdown 
%LINK-5-CHANGED: Interface Vlan1, changed state to up
S1(config)#end
```
### Финальная конфигурация
```
S1#show  run
Building configuration...
Current configuration : 1292 bytes
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
 ip address 192.168.1.2 255.255.255.0
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
 password 7 0822404F1A0A
 login
line vty 5 15
 login
!
!
!
!
end
```
## Настройка PC-A
```
Задаем Ip-address 192.168.1.10 и маску 255.255.255.0 на PC-A через интерфейс компьютера.
```

## Проверка подключения

### Проверяем сквозное соединение с помощью утилиты ping с PC-A до PC-A
![alt-текст](https://github.com/V1RaJ97/OTUS-NE/blob/5c1463ab453dff03e0a0fb4fc45ab1b6f961e0a6/Labs/Lab01/ping.png)

Результаты ping 192.168.1.10 
### Проверяем сквозное соединение с помощью утилиты ping с PC-A до S1
![alt-текст](https://github.com/V1RaJ97/OTUS-NE/blob/4eda321dd9f7fe3d709dcad4469d90e66a3d0221/Labs/Lab01/ping2.png)

Результаты ping 192.168.1.2

### Проверяем доступность коммутатора S1 с компьютера PC-A с помощью подключения по Telnet

![alt-текст](https://github.com/V1RaJ97/OTUS-NE/blob/dd4699fab4a5255ee235973d341310b49ad491a9/Labs/Lab01/telnet2.png)

Подключение по Telnet к 192.168.1.2
