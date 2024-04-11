# Лабораторная работа - Конфигурация безопасности коммутатора 
## Топология
![alt lext](https://github.com/V1RaJ97/OTUS-NE/blob/6922f1da93b805877a8d3bd7729058cc0962078a/Labs/Lab09/%D0%A2%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F.png)

## Таблица адресации
| Устройство |  Интерфейс/VLAN  |     IP-адрес   | Маска подсети |
|:----------:|:----------------:|:--------------:|:-------------:|
|     R1     |      G0/0/1      |  192.168.10.1  | 255.255.255.0 |
|     R1     |    Loopback 0    |    10.10.1.1   | 255.255.255.0 |
|     S1     |      VLAN 10     | 192.168.10.201 | 255.255.255.0 |
|     S2     |      VLAN 10     | 192.168.10.202 | 255.255.255.0 |
|    PC-A    |        NIC       |      DHCP      | 255.255.255.0 |
|    PC-B    |        NIC       |      DHCP      | 255.255.255.0 |

## Цели
1. Настройка основного сетевого устройства
2. Настройка сетей VLAN
3. Настройки безопасности коммутатора.

## Часть 1. Настройка основного сетевого устройства
### Конфигурация маршрутизатора R1
```
R1#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES unset  administratively down down 
GigabitEthernet0/0/1   192.168.10.1    YES manual up                    up 
Loopback0              10.10.1.1       YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
```
### Настройка и проверка основных параметров коммутатора
#### Коммутатор S1
```
Switch#conf ter
Switch(config)#hostname S1
S1(config)#no ip domain-lookup
S1(config)#int VLAN 10
S1(config-if)#no shutdown 
S1(config-if)#ip address 192.168.10.201 255.255.255.0
S1(config-if)#ip default-gateway 192.168.10.1
S1(config)#exit
```
#### Коммутатор S2
```
Switch#conf ter
Switch(config)#hostname S2
S2(config)#no ip domain-lookup
S2(config)#int VLAN 10
S2(config-if)#no shutdown 
S2(config-if)#ip address 192.168.10.202 255.255.255.0
S2(config-if)#ip default-gateway 192.168.10.1
S2(config)#exit
```

## Часть 2. Настройка сетей VLAN на коммутаторах.
```
S1(config)#VLAN 10
%LINK-5-CHANGED: Interface Vlan10, changed state to up
S1(config-vlan)#name Management
S1(config-vlan)#exit
```
```
S2(config)#VLAN 10
%LINK-5-CHANGED: Interface Vlan10, changed state to up
S2(config-vlan)#name Management
S2(config-vlan)#exit
```
```
S1(config)#VLAN 333
S1(config-vlan)#name Native
S1(config-vlan)#exit
S1(config)#VLAN 999
S1(config-vlan)#name ParkingLot
S1(config-vlan)#exit
```
```
S2(config)#VLAN 333
S2(config-vlan)#name Native
S2(config-vlan)#exit
S2(config)#VLAN 999
S2(config-vlan)#name ParkingLot
S2(config-vlan)#exit
```
## Часть 3. Настройки безопасности коммутатора.
```
S2(config)#int fa0/1
S2(config-if)#switchport mode trunk
S2(config-if)#switchport trunk native vlan 333
S2(config-if)#shutdown 
S2(config-if)#no shutdown

Производим те же действия на S1
```
```
S1#show interface trunk
Port        Mode         Encapsulation  Status        Native vlan
Fa0/1       on           802.1q         trunking      333

Port        Vlans allowed on trunk
Fa0/1       1-1005

Port        Vlans allowed and active in management domain
Fa0/1       1,10,333,999

Port        Vlans in spanning tree forwarding state and not pruned
Fa0/1       1,10,333,999
```
```
S1(config)#int fa0/1
S1(config-if)#switchport nonegotiate 
S1(config-if)#shutdown 
S1(config-if)#no shutdown 
S1(config)#exit
```
```
S1#show interfaces f0/1 switchport | include Negotiation
Negotiation of Trunking: Off
S2#show interfaces f0/1 switchport | include Negotiation
Negotiation of Trunking: Off
```
### Настройка портов доступа
```
S1(config)#int range fa0/5-6
S1(config-if-range)#switchport mode access 
S1(config-if-range)#switchport access vlan 10
S1(config-if-range)#shutdown 
S1(config-if-range)#no shutdown 
S1(config-if-range)#exit

То же самое делаем с портом fa0/18 на S2
```
```
S2#show vlan brief 

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/2, Fa0/3, Fa0/4, Fa0/5
                                                Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                Fa0/10, Fa0/11, Fa0/12, Fa0/13
                                                Fa0/14, Fa0/15, Fa0/16, Fa0/17
                                                Fa0/19, Fa0/20, Fa0/21, Fa0/22
                                                Fa0/23, Fa0/24, Gig0/1, Gig0/2
10   Management                       active    Fa0/18
333  Native                           active    
999  ParkingLot                       active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active
```
### Безопасность неиспользуемых портов коммутатора
```
```
