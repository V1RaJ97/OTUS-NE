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

R1(config)#interface gi0/0/1
R1(config-if)#no shutdown 
R1(config-if)#ip address 192.168.1.1 255.255.255.0
R1(config-if)#end

R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
#### Финальная конфигурация R1
```
R1#show running-config 
Building configuration...

Current configuration : 786 bytes
!
version 15.4
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname R1
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1
!
ip cef
no ipv6 cef
!
no ip domain-lookup
!
spanning-tree mode pvst
!
interface GigabitEthernet0/0/0
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface GigabitEthernet0/0/1
 ip address 192.168.1.1 255.255.255.0
 duplex auto
 speed auto
!
interface Vlan1
 no ip address
 shutdown
!
ip classless
!
ip flow-export version 9
!
no cdp run
!
banner motd ^C Unauthorized access is strictly prohibited ^C
!
line con 0
 password 7 0822455D0A16
 logging synchronous
 login
!
line aux 0
!
line vty 0 4
 password 7 0822455D0A16
 login
!
end
```
#### Резульаты эхо-запроса с PC-A до R1
```
Pinging 192.168.1.1 with 32 bytes of data:

Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
## Часть 2. Настройка маршрутизатора для доступа по протоколу SSH
```
Задаем домен и создаем ключ шифрования:

R1(config)#ip domain name otus.ru
R1(config)#crypto key generate rsa
The name for the keys will be: R1.otus.ru
Choose the size of the key modulus in the range of 360 to 2048 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]

*Mar 1 0:15:59.15: %SSH-5-ENABLED: SSH 1.99 has been enabled
R1(config)#ip ssh version 2
R1(config)#
```
```
Создаем локального пользователя и включем протокло SSH на линиях VTY
R1(config)#username admin secret Adm1nP@55
R1(config)#line vty 0 4
R1(config-line)#login local 
R1(config-line)#transport input ssh 
R1(config-line)#
```
### Проверка подключения с PC-A к R1 через SSH
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/63ffa778e1cf06ce5aeb58d6e4208ec7f3faa8f4/Labs/Lab05/TestSSH.png)
```
Соединение по SSH установлено
```
## Часть 3. Настройка коммутатора S1 для доступа по протоколу SSH
### Финальная конфигурация коммутатора
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
!
banner motd ^C Unauthorized access is strictly prohibited ^C
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
end
```
### Настройка доступа по SSH на коммутаторе S1
```
S1(config)#ip domain name otus.ru
S1(config)#crypto key generate rsa
The name for the keys will be: S1.otus.ru
Choose the size of the key modulus in the range of 360 to 2048 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
S1(config)#ip ssh version 2
S1(config)#username admin secret Adm1nP@55
S1(config)#line vty 0 4
S1(config-line)#login local
S1(config-line)#transport input ssh
S1(config-line)#end
```
### Проверка подключения по SSH с PC-A к S1
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/07071a9590f94b727e7cd3ef8149ceaa3c9ff94c/Labs/Lab05/TestSSH2.png)
```
Соединение по SSH установлено.
```
## Часть 4. Настройка протокола SSH с использованием интерфейса командной строки (CLI) коммутатора
### Параметры SSH
```
S1#ssh?
ssh  
S1#ssh
```
### Подключение по SSH S1 до R1
```
S1#ssh -l admin 192.168.1.1

Password: 

 Unauthorized access is strictly prohibited 

R1>
```
### Переключение между сеансами S1 и R1
```
R1>
S1#
S1#
[Resuming connection 1 to 192.168.1.1 ... ]
R1>exit
[Connection to 192.168.1.1 closed by foreign host]
S1#
```
## Вопрос для повторения

### Как предоставить доступ к сетевому устройству нескольким пользователям, у каждого из которых есть собственное имя пользователя?

Необходимо добавить имя каждого пользователя и пароль в локальную базу данных с помощью команды username.

