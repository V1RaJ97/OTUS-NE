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
|     10     | Management  |       S1,S2: VLAN 10        |
|     20     |    Sales    |          S1: F0/6           |
|     30     |  Operations |          S2: F0/18          |
|    999     | Parking_Lot | С1: F0/2-4, F0/7-24, G0/1-2 |
|    999     | Parking_Lot |С2: F0/2-17, F0/19-24, G0/1-2|
|   1000     |     Own     |_____________________________|
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
#### Конфигурация R1
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
Switch>enable
Switch#conf ter
Switch(config)#hostname S1
S1(config)#service password-encryption 
S1(config)#enable secret class
S1(config)#banner motd 1 Unauthorized access is strictly prohibited 1
S1(config)#no ip domain-lookup

S1(config)#line console 0
S1(config-line)#logging synchronous 
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end

S1(config)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end

S1#clock set 21:6:58 3 march 2024
S1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
```
Проделываем те же действия на коммутаторе S2
```
## Часть 2. Создание сетей VLAN и назначение портов коммутатора
### Создание и настройка сетей VLAN на коммутаторах S1 и S2.
#### S1
```
S1(config)#vlan 10
S1(config-vlan)#name Management
S1(config-vlan)#exit
S1(config)#vlan 20
S1(config-vlan)#name Sales
S1(config-vlan)#exit
S1(config)#vlan 30
S1(config-vlan)#name Operations
S1(config-vlan)#exit
S1(config)#vlan 999
S1(config-vlan)#name Parking_Lot
S1(config-vlan)#exit
S1(config)#vlan 1000 
S1(config-vlan)#name Own
S1(config-vlan)#exit
S1(config)#ip default-gateway 192.168.10.1
```
#### S2
```
S2(config)#vlan 10
S2(config-vlan)#name Management
S2(config-vlan)#exit
S2(config)#vlan 20
S2(config-vlan)#name Sales
S2(config-vlan)#exit
S2(config)#vlan 30 
S2(config-vlan)#name Operations
S2(config-vlan)#exit
S2(config)#vlan 999
S2(config-vlan)#name Parking_Lot
S2(config-vlan)#exit
S2(config)#vlan 1000
S2(config-vlan)#name Own
S2(config-vlan)#exit
S2(config)#ip default-gateway 192.168.10.1
```

### Перевод неипользуемых портов в Parking_Lot
#### Коммутатор S1
```
S1(config)#interface range f0/2-4
S1(config-if-range)#switchport mode access 
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#shutdown 
S1(config-if-range)#exit
```
```
S1(config)#interface range f0/7-24
S1(config-if-range)#switchport mode access 
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#shutdown
S1(config-if-range)#exit
```
```
S1(config)#interface range g0/1-2
S1(config-if-range)#switchport mode trunk 
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#shutdown 
S1(config-if-range)#exit
```
#### Коммутатор S2
```
S2(config)#interface range f0/2-17
S2(config-if-range)#switchport mode access 
S2(config-if-range)#switchport access vlan 999
S2(config-if-range)#shutdown 
S2(config-if-range)#exit
```
```
S2(config)#interface range f0/19-24
S2(config-if-range)#switchport mode access 
S2(config-if-range)#sw access vlan 999
S2(config-if-range)#shutdown 
S2(config-if-range)#exit
```
```
S2(config)#interface range g0/1-2
S2(config-if-range)#switchport mode trunk 
S2(config-if-range)#switchport access vlan 999
S2(config-if-range)#shutdown 
S2(config-if-range)#exit
```
### Настройка VLAN на интерфейсах
#### Коммутатор S1
```
S1(config)#int vlan 10
S1(config-if)#ip address 192.168.10.11 255.255.255.0
S1(config-if)#no shutdown 
S1(config-if)#end
```

```
S1(config)#int f0/6
S1(config-if)#switchport mode access 
S1(config-if)#sw access vlan 20
S1(config-if)#no shutdown 
S1(config-if)#exit
```
```
S1#show vlan br

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1, Fa0/5
10   Management                       active    
20   Sales                            active    Fa0/6
30   Operations                       active    
999  Parking_Lot                      active    Fa0/2, Fa0/3, Fa0/4, Fa0/7
                                                Fa0/8, Fa0/9, Fa0/10, Fa0/11
                                                Fa0/12, Fa0/13, Fa0/14, Fa0/15
                                                Fa0/16, Fa0/17, Fa0/18, Fa0/19
                                                Fa0/20, Fa0/21, Fa0/22, Fa0/23
                                                Fa0/24, Gig0/1, Gig0/2
1000 Own                              active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
```
#### Коммутатор S2
```
S2(config)#int vlan 10
S2(config-if)#ip address 192.168.10.12 255.255.255.0
S2(config-if)#no shutdown 
S2(config-if)#end
```

```
S2(config)#int f0/18
S2(config-if)#switchport mode access 
S2(config-if)#sw access vlan 30
S2(config-if)#no shutdown 
S2(config-if)#exit
```
```
S2#show vlan brief 

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/1
10   Management                       active    
20   Sales                            active    
30   Operations                       active    Fa0/18
999  Parking_Lot                      active    Fa0/2, Fa0/3, Fa0/4, Fa0/5
                                                Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                Fa0/10, Fa0/11, Fa0/12, Fa0/13
                                                Fa0/14, Fa0/15, Fa0/16, Fa0/17
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24, Gig0/1, Gig0/2
1000 Own                              active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
```
## Часть 3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами
### Коммутатор S1
```
S1(config)#int fa0/1
S1(config-if)#switchport mode trunk 
S1(config-if)#switchport trunk native vlan 1000
S1(config-if)#no shutdown 
S1(config-if)#switchport trunk allowed vlan 10,20,30,1000
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
Trunking Native Mode VLAN: 1000 (Own)
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk private VLANs: none
Operational private-vlan: none
Trunking VLANs Enabled: 10,20,30,1000
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Appliance trust: none
```
### Коммутатор S2
```
S2(config-if)#switchport mode trunk
S2(config-if)#switchport trunk native vlan 1000
S2(config-if)#no shutdown 
S2(config-if)#switchport trunk allowed vlan 10,20,30,1000
S2(config-if)#end
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
Trunking Native Mode VLAN: 1000 (Own)
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk private VLANs: none
Operational private-vlan: none
Trunking VLANs Enabled: 10,20,30,1000
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
S1(config-if)#switchport trunk native vlan 1000
S1(config-if)#no shutdown 
S1(config-if)#switchport trunk allowed vlan 10,20,30,1000
```
```
S1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
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
Trunking Native Mode VLAN: 1000 (Own)
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk private VLANs: none
Operational private-vlan: none
Trunking VLANs Enabled: 10,20,30,1000
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Appliance trust: none
```
## Часть 4. Настройка маршрутизации между сетями VLAN
### Создание и настройка подинтерфейсов
```
R1(config)#int g0/0/1
R1(config-if)#no shutdown 

R1(config)#int G0/0/1.10
R1(config-subif)#encapsulation dot1Q 10
R1(config-subif)#ip address 192.168.10.1 255.255.255.0
R1(config-subif)#no shutdown 
R1(config-subif)#end

R1(config)#int G0/0/1.20
R1(config-subif)#encapsulation dot1Q 20
R1(config-subif)#ip address 192.168.20.1 255.255.255.0
R1(config-subif)#no shutdown 
R1(config-subif)#end

R1(config)#int G0/0/1.30
R1(config-subif)#encapsulation dot1Q 30
R1(config-subif)#ip address 192.168.30.1 255.255.255.0
R1(config-subif)#no shutdown 
R1(config-subif)#end

R1(config)#int G0/0/1.1000
R1(config-subif)#encapsulation dot1Q 1000
R1(config-subif)#no shutdown 
R1(config-subif)#end
```
### Конфигурация подинтерфейсов
```
interface GigabitEthernet0/0/1
 no ip address
 duplex auto
 speed auto
!
interface GigabitEthernet0/0/1.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
!
interface GigabitEthernet0/0/1.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
!
interface GigabitEthernet0/0/1.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
!
interface GigabitEthernet0/0/1.1000
 encapsulation dot1Q 1000
 no ip address
```
