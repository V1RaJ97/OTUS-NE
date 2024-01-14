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

## 	Таблица адресации
| Устройство     | Интерфейс    | IP-адрес / префикс       | 
|:--------------:|:------------:|:------------------------:|
|        S1      |     VLAN 1   |     192.168.1.2 /24      |
|       PC-A     |      NIC     |     192.168.1.10 /24     |

## Создание сети и проверка настроек коммутатора по умолчанию
Для первоначальной настройки коммутатора необходимо подключить компьютер PC-A  к коммутатору S1 консольным кабелем и подключиться через терменал. Telnet и SSH возможно использовать только когда настраиваемый коммутатор будет доступен по сети. Сначала нужно присвоить ip адрес на интерфейсе vlan 1 и настроить vty линии. Далее активировать прикладной протокол удаленного доступа и настроить тип аутентификации, в базовом состоянии данные настройки отсутствуют.

##Настройка коммутатора

Задаем имя Switch:
 Switch>en
 Switch#conf ter
 Switch(config)#hostname S1

Устанавливаем зашифрованный пароль:
S1>en
S1#conf ter
S1(config)#enable secret cisco
S1(config)#end

Конфгурация:
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

Настройка VTY:
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

Конфигурация:
line con 0
 password 7 0822455D0A16
 logging synchronous
 login
line vty 0 4
 password 7 0822455D0A16
 login
line vty 5 15
 login
