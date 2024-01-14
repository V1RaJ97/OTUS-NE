з# 	Лабораторная работа. Базовая настройка коммутатора 
##	Задание
1. Проверка конфигурации коммутатора по умолчанию
2. Создание сети и настройка основных параметров устройства
-	Настройте базовые параметры коммутатора.
-	Настройте IP-адрес для ПК.
3. Проверка сетевых подключений
-	Отобразите конфигурацию устройства.
-	Протестируйте сквозное соединение, отправив эхо-запрос.
-	Протестируйте возможности удаленного управления с помощью Telnet.

## 	Таблица адресации
| Устройство     | Интерфейс    | IP-адрес / префикс       | 
|:--------------:|:------------:|:------------------------:|
|        S1      |     VLAN 1   |     192.168.1.2 /24      |
|       PC-A     |      NIC     |     192.168.1.10 /24     |

## Создание сети и проверка настроек коммутатора по умолчанию
Для первоначальной настройки коммутатора необходимо подключить компьютер PC-A  к коммутатору S1 консольным кабелем и подключиться через терменал. Telnet и SSH возможно использовать только когда настраиваемый коммутатор будет доступен по сети. Сначала нужно присвоить ip адрес на интерфейсе vlan 1 и настроить vty линии. Далее активировать прикладной протокол удаленного доступа и настроить тип аутентификации, в базовом состоянии данные настройки отсутствуют.

## Настройка коммутатора

### Первоначальная настройка Switch:
Switch>en

Switch#conf ter

Switch(config)#hostname S1

S1(config)#service password-encryption

S1(config)#enable secret class

S1(config)#banner motd 1 Unauthorized access is strictly prohibited 1

### Устанавливаем зашифрованный пароль:

S1>en

S1#conf ter

S1(config)#enable secret cisco

S1(config)#line console 0

S1(config-line)#logging synchronous 


### Конфгурация:
S1#show running-config 

Building configuration...

Current configuration : 1195 bytes

version 15.0

no service timestamps log datetime msec

no service timestamps debug datetime msec

no service password-encryption

hostname S1

enable secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0

no ip domain-lookup

spanning-tree mode pvst

spanning-tree extend system-id


### Настройка VTY:
S1>en

S1#conf ter

S1(config)#line vty 0 4

S1(config-line)#pass

S1(config-line)#password cisco

S1(config-line)#login

S1(config-line)#end

S1#conf ter

S1(config)#service password-encryption

S1(config)#end

### Конфигурация:
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


### Настройка Startup-config:
S1>

S1>en

S1#copy running-config startup-config 

Destination filename [startup-config]? 

Building configuration...

[OK]


### Настройка VLAN1:

S1#conf ter

S1(config)#interface vlan 1

S1(config-if)#ip address 192.168.1.2 255.255.255.0

S1(config-if)#no shutdown 

%LINK-5-CHANGED: Interface Vlan1, changed state to up

S1(config)#end

### Финальная конфигурация
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
## Проверка подключения
