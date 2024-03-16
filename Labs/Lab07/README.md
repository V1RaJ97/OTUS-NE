# Лабораторная работа. Развертывание коммутируемой сети с резервными каналами
## Топология
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/e1087a19b35e65d7d60702d9a9711ab30e438e6e/Labs/Lab07/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0%20%D1%81%D0%B5%D1%82%D0%B8.png)

## Таблица адресации
| Устройство |  Интерфейс  |   IP-адрес  | Маска подсети |
|:----------:|:-----------:|:-----------:|:-------------:|
|     S1     |    VLAN 1   | 192.168.1.1 | 255.255.255.0 |
|     S2     |    VLAN 1   | 192.168.1.2 | 255.255.255.0 |
|     S3     |    VLAN 1   | 192.168.1.3 | 255.255.255.0 |

## Задачи
1. Создание сети и настройка основных параметров устройства
2. Выбор корневого моста
3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов
4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов

## Часть 1:	Создание сети и настройка основных параметров устройства
### Настройка базовых параметров каждого коммутатора.
```
Switch(config)#hostname S1
S1(config)#enable secret class
S1(config)#service password-encryption
S1(config)#banner motd 1 Unauthorized access is strictly prohibited 1
S1(config)#no ip domain-lookup
```
```
S1(config)#line console 0
S1(config-line)#password cisco
S1(config-line)#logging synchronous
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
S1(config)#int VLAN 1
S1(config-if)#no shutdown
S1(config-if)#ip address 192.168.1.1 255.255.255.0
S1(config-if)#end
```
```
S1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
```
Аналогично настраиваем коммутаторы S2 и S3, настриваем интерфейс VLAN 1 исходя из таблицы адресации
```
#### Финальная конфигурация на примере S3
```
S3#show running-config 
Building configuration...

Current configuration : 1313 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname S3
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
 ip address 192.168.1.3 255.255.255.0
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
 logging synchronous
 login
line vty 5 15
 login
!
end
```
#### Проверка связи между коммутаторами
##### Эхо-запрос с коммутатора S1 на S2
```
S1>ping 192.168.1.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
```
##### Эхо-запрос с коммутатора S1 на S3
```
S1>ping 192.168.1.3
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/1 ms
```
##### Эхо-запрос с коммутатора S2 на S3
```
S2>ping 192.168.1.3
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
```
## Часть 2:	Определение корневого моста
### Настройка портов на коммутаторах
```
S1(config)#interface range fa0/1-24
S1(config-if-range)#shutdown
S1(config-if-range)#exit
S1(config)#interface range g0/1-2
S1(config-if-range)#shutdown 
```
```
S1(config)#interface range fa0/1-4
S1(config-if-range)#switchport mode trunk 
S1(config-if-range)#end
S1(config)#int fa0/2
S1(config-if)#no shutdown 
S1(config-if)#exit
S1(config)#int fa0/4
S1(config-if)#no shutdown 
S1(config-if)#exit
```
```
Аналогичные действия выполняем на коммутаторах S2 и S3
```
### Данные протокола spanning-tree
#### Коммутатор S1
```
S1# show spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0007.EC5A.525D
             Cost        19
             Port        2(FastEthernet0/2)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     000A.4114.BBC8
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Root FWD 19        128.2    P2p
Fa0/4            Desg FWD 19        128.4    P2p
```
#### Коммутатор S2
```
S2#show spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0007.EC5A.525D
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0007.EC5A.525D
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/4            Desg FWD 19        128.4    P2p
Fa0/2            Desg FWD 19        128.2    P2p
```
#### Коммутатор S3
```
S3#show spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0007.EC5A.525D
             Cost        19
             Port        2(FastEthernet0/2)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     00D0.BC0D.3305
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Root FWD 19        128.2    P2p
Fa0/4            Altn BLK 19        128.4    P2p
```
### Состояние портов
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/47a44e6a42d300ab43b77b42327aaff216317755/Labs/Lab07/%D0%A1%D0%BE%D1%81%D1%82%D0%BE%D1%8F%D0%BD%D0%B8%D0%B5%20%D0%BF%D0%BE%D1%80%D1%82%D0%BE%D0%B2.png)
### Ответы на вопросы

1. Какой коммутатор является корневым мостом? Ответ: Коммутатор S2.
2. Почему этот коммутатор был выбран протоколом spanning-tree в качестве корневого моста? Ответ: Потому что коммутатор S2 имеет самое низкое значение MAC-адреса.
3. Какие порты на коммутаторе являются корневыми портами? Ответ: порт ведущий к корневому коммутатору с наименьшим порядковым номером (fa0/2)
4. Какие порты на коммутаторе являются назначенными портами? Ответ: назначенным портом выбирается порт, имеющий лучшую стоимость в данном сегменте. У корневого коммутатора все порты — назначенные.
5. Какой порт отображается в качестве альтернативного и в настоящее время заблокирован? Ответ: порт Fa0/4 на коммутаторе S3
6. Почему протокол spanning-tree выбрал этот порт в качестве невыделенного (заблокированного) порта? Ответ: Потому что у него замое высокое значение Bridge ID

