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
S1(config)#int range fa0/2-4
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#shutdown 
S1(config-if-range)#exit
S1(config)#int range fa0/7-24
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#shutdown 
S1(config-if-range)#exit
S1(config)#int range gi0/1-2
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#shutdown 
S1(config-if-range)#exit
```
```
S1#show interfaces status 
Port      Name               Status       Vlan       Duplex  Speed Type
Fa0/1                        connected    trunk      auto    auto  10/100BaseTX
Fa0/2                        disabled 999        auto    auto  10/100BaseTX
Fa0/3                        disabled 999        auto    auto  10/100BaseTX
Fa0/4                        disabled 999        auto    auto  10/100BaseTX
Fa0/5                        connected    10         auto    auto  10/100BaseTX
Fa0/6                        connected    10         auto    auto  10/100BaseTX
Fa0/7                        disabled 999        auto    auto  10/100BaseTX
Fa0/8                        disabled 999        auto    auto  10/100BaseTX
Fa0/9                        disabled 999        auto    auto  10/100BaseTX
Fa0/10                       disabled 999        auto    auto  10/100BaseTX
Fa0/11                       disabled 999        auto    auto  10/100BaseTX
Fa0/12                       disabled 999        auto    auto  10/100BaseTX
Fa0/13                       disabled 999        auto    auto  10/100BaseTX
Fa0/14                       disabled 999        auto    auto  10/100BaseTX
Fa0/15                       disabled 999        auto    auto  10/100BaseTX
Fa0/16                       disabled 999        auto    auto  10/100BaseTX
Fa0/17                       disabled 999        auto    auto  10/100BaseTX
Fa0/18                       disabled 999        auto    auto  10/100BaseTX
Fa0/19                       disabled 999        auto    auto  10/100BaseTX
Fa0/20                       disabled 999        auto    auto  10/100BaseTX
Fa0/21                       disabled 999        auto    auto  10/100BaseTX
Fa0/22                       disabled 999        auto    auto  10/100BaseTX
Fa0/23                       disabled 999        auto    auto  10/100BaseTX
Fa0/24                       disabled 999        auto    auto  10/100BaseTX
Gig0/1                       disabled 999        auto    auto  10/100BaseTX
Gig0/2                       disabled 999        auto    auto  10/100BaseTX
```
```
S2(config)#int range fa0/2-17
S2(config-if-range)#switchport access vlan 999
S2(config-if-range)#shutdown 
S2(config-if-range)#exit
S2(config)#int range fa0/19-24
S2(config-if-range)#switchport access vlan 999
S2(config-if-range)#shutdown 
S2(config-if-range)#exit
S2(config)#int range gi0/1-2
S2(config-if-range)#switchport access vlan 999
S2(config-if-range)#shutdown 
S2(config-if-range)#exit
```
```
S2#show interfaces status 
Port      Name               Status       Vlan       Duplex  Speed Type
Fa0/1                        connected    trunk      auto    auto  10/100BaseTX
Fa0/2                        disabled 999        auto    auto  10/100BaseTX
Fa0/3                        disabled 999        auto    auto  10/100BaseTX
Fa0/4                        disabled 999        auto    auto  10/100BaseTX
Fa0/5                        disabled 999        auto    auto  10/100BaseTX
Fa0/6                        disabled 999        auto    auto  10/100BaseTX
Fa0/7                        disabled 999        auto    auto  10/100BaseTX
Fa0/8                        disabled 999        auto    auto  10/100BaseTX
Fa0/9                        disabled 999        auto    auto  10/100BaseTX
Fa0/10                       disabled 999        auto    auto  10/100BaseTX
Fa0/11                       disabled 999        auto    auto  10/100BaseTX
Fa0/12                       disabled 999        auto    auto  10/100BaseTX
Fa0/13                       disabled 999        auto    auto  10/100BaseTX
Fa0/14                       disabled 999        auto    auto  10/100BaseTX
Fa0/15                       disabled 999        auto    auto  10/100BaseTX
Fa0/16                       disabled 999        auto    auto  10/100BaseTX
Fa0/17                       disabled 999        auto    auto  10/100BaseTX
Fa0/18                       connected    10         auto    auto  10/100BaseTX
Fa0/19                       disabled 999        auto    auto  10/100BaseTX
Fa0/20                       disabled 999        auto    auto  10/100BaseTX
Fa0/21                       disabled 999        auto    auto  10/100BaseTX
Fa0/22                       disabled 999        auto    auto  10/100BaseTX
Fa0/23                       disabled 999        auto    auto  10/100BaseTX
Fa0/24                       disabled 999        auto    auto  10/100BaseTX
Gig0/1                       disabled 999        auto    auto  10/100BaseTX
Gig0/2                       disabled 999        auto    auto  10/100BaseTX
```
### Документирование и реализация функций безопасности порта.
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/e6ab6bd195fdaa73b017596d8bff400a0cfdda52/Labs/Lab09/%D0%94%D0%BE%D0%BA%D1%83%D0%BC%D0%B5%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5.png)

```
S1(config)#int fa0/6
S1(config-if)#switchport port-security
S1(config-if)#switchport port-security violation restrict 
S1(config-if)#switchport port-security aging time 60
S1(config-if)#switchport port-security maximum 3
S1(config-if)#switchport port-security aging type inactivity
                                              ^
% Invalid input detected at '^' marker.
```
```
S1#show port-security interface f0/6
Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Restrict
Aging Time                 : 60 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 3
Total MAC Addresses        : 1
Configured MAC Addresses   : 0
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 0030.F230.E750:10
Security Violation Count   : 0
```
```
S2(config)#int fa0/18
S2(config-if)#switchport port-security 
S2(config-if)#switchport port-security violation protect 
S2(config-if)#switchport port-security aging time 60
S2(config-if)#switchport port-security maximum 2
S2(config-if)#switchport port-security mac-address sticky


S2(config-if)#exit
```
```
S2#show port-security interface f0/18
Port Security              : Enabled
Port Status                : Secure-up
Violation Mode             : Protect
Aging Time                 : 60 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 2
Total MAC Addresses        : 1
Configured MAC Addresses   : 0
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 0001.9776.75C1:10
Security Violation Count   : 0
```
```
S2#show port-security address
               Secure Mac Address Table
-----------------------------------------------------------------------------
Vlan    Mac Address       Type                          Ports   Remaining Age
                                                                   (mins)
----    -----------       ----                          -----   -------------
10	0001.9776.75C1	DynamicConfigured	FastEthernet0/18		-
-----------------------------------------------------------------------------
Total Addresses in System (excluding one mac per port)     : 0
Max Addresses limit in System (excluding one mac per port) : 1024
```
### Реализация безопасности DHCP snooping
```
S2(config)#ip dhcp snooping
S2(config)#ip dhcp snooping VLAN 10
S2(config)#exit
```
```
S2(config)#int fa0/1
S2(config-if)#ip dhcp snooping trust 
S2(config-if)#exit
S2(config)#int fa0/18
S2(config-if)#ip dhcp snooping limit rate 5
S2(config-if)#exit
```
```
S2#show ip dhcp snooping
Switch DHCP snooping is enabled
DHCP snooping is configured on following VLANs:
10
Insertion of option 82 is enabled
Option 82 on untrusted port is not allowed
Verification of hwaddr field is enabled
Interface                  Trusted    Rate limit (pps)
-----------------------    -------    ----------------
FastEthernet0/1            yes        unlimited       
FastEthernet0/18           no         5   
```
```
C:\>ipconfig /release

   IP Address......................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: 0.0.0.0
   DNS Server......................: 0.0.0.0

C:\>ipconfig /renew
DHCP request failed.
```
```
S2#show ip dhcp snooping binding 
MacAddress          IpAddress        Lease(sec)  Type           VLAN  Interface
------------------  ---------------  ----------  -------------  ----  -----------------
Total number of bindings: 0
```
### Реализация PortFast и BPDU Guard
```
S1(config)#int fa0/5
S1(config-if)#spanning-tree portfast
S1(config-if)#spanning-tree bpduguard enable 
S1(config-if)#exit
S1(config)#int fa0/6
S1(config-if)#spanning-tree portfast
S1(config-if)#spanning-tree bpduguard enable 
S1(config-if)#exit
S1(config)#spanning-tree portfast bpduguard default
```
```
S2(config)#int fa0/18
S2(config-if)#spanning-tree portfast
%Warning: portfast should only be enabled on ports connected to a single
host. Connecting hubs, concentrators, switches, bridges, etc... to this
interface  when portfast is enabled, can cause temporary bridging loops.
Use with CAUTION

%Portfast has been configured on FastEthernet0/18 but will only
have effect when the interface is in a non-trunking mode.
S2(config-if)#exit
S2(config)#spanning-tree portfast default
S2(config)#exit
```
```
S1#show spanning-tree interface f0/6 detail
Port 6 (FastEthernet0/6) of VLAN0010 is designated forwarding
  Port path cost 19, Port priority 128, Port Identifier 128.6
  Designated root has priority 32778, address 0001.C954.A779
  Designated bridge has priority 32778, address 0030.F28B.797B
  Designated port id is 128.6, designated path cost 19
  Timers: message age 16, forward delay 0, hold 0
  Number of transitions to forwarding state: 1
  The port is in the portfast mode
  Link type is point-to-point by default
```
### Проверьте наличие сквозного ⁪подключения
```
Пинг с S2 до PC-A
S2#ping 192.168.10.11

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.10.11, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 0/0/1 ms
```
```
Пинг с S2 до G0/0/1 на R1
S2#ping 192.168.10.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.10.1, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 0/0/0 ms
```
```
Пинг с S2 до LoopBack 0 на R1
S2#ping 10.10.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.10.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/3/8 ms
```
## Вопросы для повторения
С точки зрения безопасности порта на S2, почему нет значения таймера для оставшегося возраста в минутах, когда было сконфигурировано динамическое обучение - sticky?

Что касается безопасности порта на S2, если вы загружаете скрипт текущей конфигурации на S2, почему порту 18 на PC-B никогда не получит IP-адрес через DHCP?

Что касается безопасности порта, в чем разница между типом абсолютного устаревания и типом устаревание по неактивности?
Ответ: При абсолютном утаревании адреса порта очищаются по истечении установленного времени, а при устаревании по неактивности только когда они
неактивны в течение установленного времени.
