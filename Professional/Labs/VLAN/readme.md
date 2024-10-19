# Лабораторная работа - настройка маршрутизации между VLAN-сетями Router-on-a-Stick
## Схема сети
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/6001e275ef472672e0e3d10665f01b51e62f0bb7/Professional/Labs/VLAN/vlan.png)
## Таблица адресации
| Устройство |  Интерфейс  |   IP-адрес   | Маска подсети | Шлюз по умолчанию |
|:----------:|:-----------:|:------------:|:-------------:|:-----------------:|
|     R1     |   G0/0/1.3  |  192.168.3.1 | 255.255.255.0 | _________________ |
|     R1     |   G0/0/1.4  |  192.168.4.1 | 255.255.255.0 | _________________ |
|     R1     |   G0/0/1.8  | ____________ | _____________ | _________________ |
|     S1     |    VLAN 3   | 192.168.3.11 | 255.255.255.0 |    192.168.3.1    |
|     S2     |    VLAN 3   | 192.168.3.12 | 255.255.255.0 |    192.168.3.1    |
|    PC-A    |     NIC     | 192.168.3.3  | 255.255.255.0 |    192.168.3.1    |
|    PC-A    |     NIC     | 192.168.4.3  | 255.255.255.0 |    192.168.4.1    |

## Таблица VLAN
|    VLAN    |     Имя     |    Назначенный интерфейс    |
|:----------:|:-----------:|:---------------------------:|
|     3      | Management  |S1:VLAN 3; S2:VLAN 3; S1:F0/6|
|     4      |  Operations |          S2: F0/18          |
|     7      | Parking_Lot | S1: F0/2-4, F0/7-24, G0/1-2 |
|     7      | Parking_Lot |S2: F0/2-17, F0/19-24, G0/1-2|
|     8      |   Native    |_____________________________|
## Задачи
1. Создание сети и настройка основных параметров устройства
2. Создание сетей VLAN и назначение портов коммутатора
3. Настройка транка 802.1Q между коммутаторами.
4. Настройка маршрутизации между сетями VLAN
5. Проверка, что маршрутизация между VLAN работает

## Цели
1. Создание сети и настройка основных параметров устройства
2. Создание VLAN и назначение портов коммутатора
3. Настройка транка 802.1Q между коммутаторами
4. Настройка маршрутизации между VLAN на маршрутизаторе Часть
5. Проверка работы маршрутизации между VLAN

## Часть 1: Создание сети и настройка основных параметров устройства
### Базовая настройка маршрутизатора R1
```
Router(config)#hostname R1
R1(config)#no ip domain-lookup
R1(config)#enable secret class
R1(config)#service password-encryption 
R1(config)#banner motd 1 Unauthorized access is strictly prohibited 1
```
```
R1(config)#line console 0
R1(config-line)#logging synchronous 
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
```
```
R1(config)#line vty 0 4
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#end
```
```
R1#clock set 16:15:00 19 october 2024
R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
### Конфигурация R1
```
R1#show running-config 
Building configuration...

Current configuration : 760 bytes
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
 no ip address
 duplex auto
 speed auto
 shutdown
!
interface Vlan1
 no ip address
 shutdown
!
ip classless
!
ip flow-export version 9
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
### Настройка базовых параметров коммутаторов S1 и S2
```
S1(config)#hostname S1
S1(config)#service password-encryption 
S1(config)#enable secret class
S1(config)#banner motd 1 Unauthorized access is strictly prohibited 1
S1(config)#no ip domain-lookup
```
```
S1(config)#line console 0
S1(config-line)#logging synchronous 
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
```
```
S1(config)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
```
```
S1#clock set 16:20:00 19 october 2024
S1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
```
Производим аналогичную настройку на S2
```
### Конфигурация S1/S2
```
S2#show running-config 
Building configuration...

Current configuration : 1279 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname S2
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
 no ip address
 shutdown
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
!
end
```
## Часть 2: Создание VLAN и назначение портов коммутатора
### Создание и настройка VLAN на коммутаторах S1 и S2
#### S1
```
S1(config)#vlan 3
S1(config-vlan)#name Management
S1(config-vlan)#exit
S1(config)#vlan 4
S1(config-vlan)#name Operations
S1(config-vlan)#exit
S1(config)#vlan 7
S1(config-vlan)#name Parking_Lot
S1(config-vlan)#exit
S1(config)#vlan 8
S1(config-vlan)#name Native
S1(config-vlan)#exit
S1(config)#ip default-gateway 192.168.3.1
```
```
Аналогичным образом настраиваем на S2
```
### Перевод неипользуемых портов на коммутаторах в Parking_Lot
#### S1
```
S1(config)#interface range f0/2-4
S1(config-if-range)#switchport mode access 
S1(config-if-range)#switchport access vlan 7
S1(config-if-range)#shutdown
S1(config-if-range)#exit
S1(config)#interface range f0/7-24
S1(config-if-range)#switchport mode access 
S1(config-if-range)#switchport access vlan 7
S1(config-if-range)#shutdown 
S1(config-if-range)#exit
S1(config)#interface range g0/1-2
S1(config-if-range)#switchport mode access 
S1(config-if-range)#switchport access vlan 7
S1(config-if-range)#shutdown 
S1(config-if-range)#exit
```
#### S2
```
S2(config)#interface range f0/2-17
S2(config-if-range)#switchport mode access 
S2(config-if-range)#switchport access vlan 7
S2(config-if-range)#shutdown 
S2(config-if-range)#exit
S2(config)#interface range f0/19-24
S2(config-if-range)#switchport mode access
S2(config-if-range)#switchport access vlan 7
S2(config-if-range)#shutdown 
S2(config-if-range)#exit
S2(config)#interface range g0/1-2
S2(config-if-range)#switchport mode access
S2(config-if-range)#switchport access vlan 7
S2(config-if-range)#shutdown 
S2(config-if-range)#exit
```

### Настройка VLAN на интерфейсах
#### S1
```
S1(config)#int vlan 3
S1(config-if)#ip address 192.168.3.11 255.255.255.0
S1(config-if)#exit
S1(config)#int f0/6
S1(config-if)#switchport mode access 
S1(config-if)#switchport access vlan 3
S1(config-if)#end
S1#show vlan brief 

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/5
3    Management                       active    Fa0/6
4    Operations                       active    
7    Parking_Lot                      active    Fa0/2, Fa0/3, Fa0/4, Fa0/7
                                                Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gig0/1, Gig0/2
8    Native                           active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active  
```
#### S2
```
S2(config)#int vlan 3
S2(config-if)#ip address 192.168.3.12 255.255.255.0
S2(config-if)#exit
S2(config)#int f0/18
S2(config-if)#switchport mode access 
S2(config-if)#sw access vlan 4
S2(config-if)#no shutdown 
S2(config-if)#end
S2#show vlan brief 

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1
3    Management                       active    
4    Operations                       active    Fa0/18
7    Parking_Lot                      active    Fa0/2, Fa0/3, Fa0/4, Fa0/5
                                                Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                Fa0/10, Fa0/11, Fa0/12, Fa0/13
                                                Fa0/14, Fa0/15, Fa0/16, Fa0/17
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24, Gig0/1, Gig0/2
8    VLAN0008                         active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active  
```
## Часть 3. Настройка магистрального канала  802.1Q между коммутаторами
### S1
```
S1(config)#int fa0/1
S1(config-if)#switchport mode trunk
S1(config-if)#switchport trunk native vlan 8
S1(config-if)#switchport trunk allowed vlan 3,4,8
S1(config-if)#exit
```
#### Информация об интерфейсе fa0/1 на коммутаторе S1
```
S1#show interfaces fa0/1 switchport 
Name: Fa0/1
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: dot1q
Negotiation of Trunking: On
Access Mode VLAN: 1 (default)
Trunking Native Mode VLAN: 8 (Native)
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk private VLANs: none
Operational private-vlan: none
Trunking VLANs Enabled: 3-4,8
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Appliance trust: none
```
### S2
```
S2(config)#int fa0/1
S2(config-if)#switchport mode trunk
S2(config-if)#switchport trunk native vlan 8
S2(config-if)#switchport trunk allowed vlan 3,4,8
S2(config-if)#exit
```
#### Информация об интерфейсе fa0/1 на коммутаторе S2
```
S2#show interfaces fa0/1 switchport 
Name: Fa0/1
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: dot1q
Negotiation of Trunking: On
Access Mode VLAN: 1 (default)
Trunking Native Mode VLAN: 8 (VLAN0008)
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk private VLANs: none
Operational private-vlan: none
Trunking VLANs Enabled: 3-4,8
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Appliance trust: none
```
### Настройка интерфейса Fa0/5 на коммутаторе S1
```
S1(config)#int fa0/5
S1(config-if)#switchport mode trunk
S1(config-if)#switchport trunk native vlan 8
S1(config-if)#switchport trunk allowed vlan 3,4,8
S1(config-if)#exit
```
#### Инфорация об интерфейсе Fa0/5 на коммутаторе S1
```
S1#show interfaces fa0/5 switchport 
Name: Fa0/5
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: down
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: dot1q
Negotiation of Trunking: On
Access Mode VLAN: 1 (default)
Trunking Native Mode VLAN: 8 (Native)
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk private VLANs: none
Operational private-vlan: none
Trunking VLANs Enabled: 3-4,8
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Appliance trust: none
```
## Часть 4. Настройка маршрутизации между сетями VLAN
### Создание и настройка сабинтерфейсов на R1
```
R1(config)#int g0/0/1
R1(config-if)#no shutdown 
R1(config-if)#exit
R1(config)#int g0/0/1.3
R1(config-subif)#encapsulation dot1Q 3
R1(config-subif)#ip address 192.168.3.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.4
R1(config-subif)#encapsulation dot1Q 4
R1(config-subif)#ip address 192.168.4.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.8
R1(config-subif)#encapsulation dot1Q 8 native
R1(config-subif)#no shutdown 
R1(config-subif)#end
```
#### Конфигурация подинтерфейсов
```
R1#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES unset  administratively down down 
GigabitEthernet0/0/1   unassigned      YES unset  up                    up 
GigabitEthernet0/0/1.3 192.168.3.1     YES manual up                    up 
GigabitEthernet0/0/1.4 192.168.4.1     YES manual up                    up 
GigabitEthernet0/0/1.8 unassigned      YES unset  up                    up 
Vlan1                  unassigned      YES unset  administratively down down
```
## Часть 5. Проверка маршрутизации между VLAN
### Ping от PC-A до шлюза по умолчанию
```
C:\>ping 192.168.3.1

Pinging 192.168.3.1 with 32 bytes of data:

Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255
Reply from 192.168.3.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.3.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

```

### Ping от PC-A до PC-B
```
C:\>ping 192.168.4.3

Pinging 192.168.4.3 with 32 bytes of data:

Reply from 192.168.4.3: bytes=32 time<1ms TTL=127
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127
Reply from 192.168.4.3: bytes=32 time<1ms TTL=127

Ping statistics for 192.168.4.3:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
### Ping от PC-A до S2
```
C:\>ping 192.168.3.12

Pinging 192.168.3.12 with 32 bytes of data:

Reply from 192.168.3.12: bytes=32 time<1ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255
Reply from 192.168.3.12: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.3.12:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
### Tracert от PC-B до PC-A
```
C:\>tracert 192.168.3.3

Tracing route to 192.168.3.3 over a maximum of 30 hops: 

  1   0 ms      0 ms      0 ms      192.168.4.1
  2   0 ms      0 ms      0 ms      192.168.3.3

Trace complete.

```
