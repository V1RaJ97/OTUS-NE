100# Лабораторная работа - Реализация DHCPv4 
## Топология
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/50260140e22db30f62ca76cf9e5e8543e1c08e15/Labs/Lab08/%D0%A2%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F(ipv4).png)
## Таблица адресации
|  Устройство  |  Интерфейс  |  IP-адрес  |  Маска подсети  | Шлюз по умолчанию |
|:------------:|:-----------:|:----------:|:---------------:|:-----------------:|
|      R1      |    G0/0/0   |  10.0.0.1  | 255.255.255.252 |                   |
|      R1      |    G0/0/1   |            |                 |                   |
|      R1      | G0/0/1.100  | 192.168.1.1| 255.255.255.192 |   192.168.1.1     |
|      R1      | G0/0/1.200  |192.168.1.65| 255.255.255.224	|   192.168.1.65    |
|      R1      | G0/0/1.1000 |            |                 |                   |
|      R2      |    G0/0     |  10.0.0.2  | 255.255.255.252 |                   |
|      R2      |    G0/0/1   |192.168.1.97| 255.255.255.240 |   192.168.1.97    |
|      S1      |   VLAN 200  |192.168.1.66| 255.255.255.224 |   192.168.1.65    |
|      S2      |    VLAN 1   |192.168.1.98| 255.255.255.240 |   192.168.1.97    |
|     PC-A     |     NIC     |    DHCP    |       DHCP      |       DHCP        |
|     PC-B     |     NIC     |    DHCP    |       DHCP      |       DHCP        |
## Таблица VLAN
|   VLAN   |     Имя     |    Назначенный интерфейс    |
|:--------:|:-----------:|:---------------------------:|
|     1    |     Нет     |         S2: F0/18           |
|    100   |   Clients   |          S1: F0/6           |
|    200   |  Management |         S1: VLAN 200        |
|    999   | Parking_Lot | S1: F0/1-4, F0/7-24, G0/1-2 |
|   1000   |   Private   |                             |
## Задачи
1. Создание сети и настройка основных параметров устройства
2. Настройка и проверка двух серверов DHCPv4 на R1
3. Настройка и проверка DHCP-ретрансляции на R2

## Часть 1.	Создание сети и настройка основных параметров устройства
### Базовая настройка маршрутизаторов R1 и R2
```
Router(config)#hostname R1
R1(config)#no ip domain-lookup
R1(config)#service password-encryption 
R1(config)#enable secret class
R1(config)#banner motd 1 Unauthorized access is strictly prohibited 1
```
```
R1(config)#line con 0
R1(config-line)#logging synchronous 
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
```
```
R1(config)#line vty 0 4
R1(config-line)#logging synchronous 
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
Проводим аналогичные настройки на R2
### Настройка маршрутизации между сетями VLAN на маршрутизаторе R1
```
R1(config)#int g0/0/1
R1(config-if)#no shutdown
R1(config-if)#exit

R1(config)#int g0/0/1.100
R1(config-subif)#encapsulation dot1Q 100
R1(config-subif)#ip address 192.168.1.1 255.255.255.192
R1(config-subif)#no shutdown 
R1(config-subif)#exit

R1(config)#int g0/0/1.200
R1(config-subif)#encapsulation dot1Q 200
R1(config-subif)#ip address 192.168.1.65 255.255.255.224
R1(config-subif)#no shutdown 
R1(config-subif)#exit

R1(config)#int g0/0/1.1000
R1(config-subif)#encapsulation dot1Q 1000 native
R1(config-subif)#no shutdown
R1(config-subif)#exit
```
#### Конфигурация подинтерфейсов на R1
```
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
!
interface GigabitEthernet0/0/1.100
 encapsulation dot1Q 100
 ip address 192.168.1.1 255.255.255.192
!
interface GigabitEthernet0/0/1.200
 encapsulation dot1Q 200
 ip address 192.168.1.65 255.255.255.224
!
interface GigabitEthernet0/0/1.1000
 encapsulation dot1Q 1000 native
 no ip address
```
### Настройка интерфейсов и статической маршрутизации на маршрутизаторах R1 и R2
```
R2(config)#int g0/0/1
R2(config-if)#no shutdown 
R2(config-if)#ip address 192.168.1.97 255.255.255.240
```
```
R2(config)#int g0/0/0
R2(config-if)#ip address 10.0.0.2 255.255.255.252
R2(config-if)#no shutdown 
R2(config-if)#exit
```
```
R1(config)#int g0/0/0
R1(config-if)#ip address 10.0.0.1 255.255.255.252
R1(config-if)#no shutdown 
R1(config-if)#exit
```
```
R1(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.2
R1(config)#exit
R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
```
R2(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.1
R2(config)#exit
R2#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
#### Реузлаьаты ping с R1 до G0/0/1 на R2
```
R1#ping 192.168.1.97

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.97, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
```
### Настройка базовых параметров коммутаторов S1 и S2
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
S1(config-line)#exit
```
```
S1(config)#line vty 0 4
S1(config-line)#logging synchronous 
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
```
```
S1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
```
Призводим аналогичные настройки на коммутаторе S2
```
### Создание и настройка VLAN на коммутаорах S1 и S2
```
S1(config)#vlan 100
S1(config-vlan)#name Clients
S1(config-vlan)#exit
S1(config)#vlan 200
S1(config-vlan)#name Management
S1(config-vlan)#exit
S1(config)#vlan 999
S1(config-vlan)#name Parking_Lot
S1(config-vlan)#exit
S1(config)#vlan 1000
S1(config-vlan)#name Private
S1(config-vlan)#exit
```
```
S1(config)#int VLAN 200
S1(config-if)#no shutdown
S1(config-if)#ip address 192.168.1.66 255.255.255.224
S1(config-if)#ip default-gateway 192.168.1.65
S1(config)#exit
```
```
S2(config)#int VLAN 1
S2(config-if)#no shutdown 
S2(config-if)#ip address 192.168.1.98 255.255.255.240
S2(config-if)#ip default-gateway 192.168.1.97
S2(config)#exit
```
```
S1(config)#interface range fa0/1-4
S1(config-if-range)#switchport mode access 
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#exit
S1(config)#interface range fa0/7-24
S1(config-if-range)#switchport mode access 
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#exit
S1(config)#interface range g0/1-2
S1(config-if-range)#switchport mode access 
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#exit
```
```
S2(config)#interface range fa0/1-4
S2(config-if-range)#shutdown 
S2(config-if-range)#exit
S2(config)#interface range fa0/6-17
S2(config-if-range)#shutdown 
S2(config-if-range)#exit
S2(config)#interface range fa0/19-24
S2(config-if-range)#shutdown
S2(config-if-range)#exit
S2(config)#interface range gi0/1-2
S2(config-if-range)#shutdown
S2(config-if-range)#exit
```
### Назначение сетей VLAN соответствующим интерфейсам коммутаторов.
```
S1(config)#int fa0/6
S1(config-if)#switchport mode access 
S1(config-if)#switchport access vlan 100
S1(config-if)#shutdown 
S1(config-if)#no shutdown 
S1(config-if)#exit
```
```
S2(config)#int fa0/18
S2(config-if)#switchport mode access 
S2(config-if)#switchport access vlan 1
S2(config-if)#shutdown 
S2(config-if)#no shutdown 
S2(config-if)#exit
```
### Настройка интерфейса S1 F0/5 в качестве транка 802.1Q.
```
S1(config)#int fa0/5
S1(config-if)#switchport mode trunk 
S1(config-if)#switchport trunk native vlan 1000
S1(config-if)#switchport trunk allowed vlan 100,200,1000
S1(config-if)#shutdown 
S1(config-if)#no shutdown 
S1(config-if)#exit
```
```
!
interface FastEthernet0/5
 switchport trunk native vlan 1000
 switchport trunk allowed vlan 100,200,1000
 switchport mode trunk
!
```
## Настройка и проверка двух серверов DHCPv4 на R1
```
R1(config)#ip dhcp pool R1_Client_LAN
R1(dhcp-config)#domain-name CCNA-lab.com
R1(dhcp-config)#network 192.168.1.0 255.255.255.192
R1(dhcp-config)#default-router 192.168.1.1
R1(dhcp-config)#ip dhcp excluded-address 192.168.1.1 192.168.1.5
```
```
R1(config)#ip dhcp pool R2_Client_LAN
R1(dhcp-config)#domain-name CCNA-lab.com
R1(dhcp-config)#network 192.168.1.96 255.255.255.240
R1(dhcp-config)#default-router 192.168.1.97
R1(dhcp-config)#exit
R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
### Проверка конфигурации сервера DHCPv4
```
R1#show ip dhcp pool

Pool R1_Client_LAN :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0 
 Total addresses                : 62
 Leased addresses               : 0
 Excluded addresses             : 1
 Pending event                  : none

 1 subnet is currently in the pool
 Current index        IP address range                    Leased/Excluded/Total
 192.168.1.1          192.168.1.1      - 192.168.1.62      0    / 1     / 62

Pool R2_Client_LAN :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0 
 Total addresses                : 14
 Leased addresses               : 0
 Excluded addresses             : 1
 Pending event                  : none

 1 subnet is currently in the pool
 Current index        IP address range                    Leased/Excluded/Total
 192.168.1.97         192.168.1.97     - 192.168.1.110     0    / 1     / 14
R1#show ip dhcp binding 
IP address       Client-ID/              Lease expiration        Type
                 Hardware address
R1#show ip dhcp binding 
IP address       Client-ID/              Lease expiration        Type
                 Hardware address

R1#show ip dhcp server statistics
                ^
% Invalid input detected at '^' marker.
```
### Попытка получить IP-адрес от DHCP на PC-A
```
C:\>ipconfig /all

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Physical Address................: 0001.C7AB.7B26
   Link-local IPv6 Address.........: FE80::201:C7FF:FEAB:7B26
   IPv6 Address....................: ::
   IPv4 Address....................: 192.168.1.2
   Subnet Mask.....................: 255.255.255.192
   Default Gateway.................: ::
                                     192.168.1.1
   DHCP Servers....................: 192.168.1.1
   DHCPv6 IAID.....................: 
   DHCPv6 Client DUID..............: 00-01-00-01-5A-4A-BA-5B-00-01-C7-AB-7B-26
   DNS Servers.....................: ::
                                     0.0.0.0
```
```
C:\>ping 192.168.1.97

Pinging 192.168.1.97 with 32 bytes of data:

Reply from 192.168.1.97: bytes=32 time<1ms TTL=254
Reply from 192.168.1.97: bytes=32 time<1ms TTL=254
Reply from 192.168.1.97: bytes=32 time<1ms TTL=254
Reply from 192.168.1.97: bytes=32 time<1ms TTL=254

Ping statistics for 192.168.1.97:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
## Настройка и проверка DHCP-ретрансляции на R2
```
R2(config)#int g0/0/1
R2(config-if)#ip helper-address 10.0.0.1
R2(config-if)#exit
R2#copy running-config startup-config
Destination filename [startup-config]? 
Building configuration...
[OK]
```
### Поучение ip-адреса для PC-B
```
C:\>ipconfig /all

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: CCNA-lab.com
   Physical Address................: 0090.0C4E.1A09
   Link-local IPv6 Address.........: ::
   IPv6 Address....................: ::
   IPv4 Address....................: 192.168.1.99
   Subnet Mask.....................: 255.255.255.240
   Default Gateway.................: ::
                                     192.168.1.97
   DHCP Servers....................: 10.0.0.1
   DHCPv6 IAID.....................: 
   DHCPv6 Client DUID..............: 00-01-00-01-9B-48-B7-B3-00-90-0C-4E-1A-09
   DNS Servers.....................: ::
                                     0.0.0.0
```
```
C:\>ping 192.168.1.1

Pinging 192.168.1.1 with 32 bytes of data:

Reply from 192.168.1.1: bytes=32 time<1ms TTL=254
Reply from 192.168.1.1: bytes=32 time<1ms TTL=254
Reply from 192.168.1.1: bytes=32 time=26ms TTL=254
Reply from 192.168.1.1: bytes=32 time=1ms TTL=254

Ping statistics for 192.168.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 26ms, Average = 6ms
```
```
R1#show ip dhcp binding 
IP address       Client-ID/              Lease expiration        Type
                 Hardware address
192.168.1.99     0090.0C4E.1A09           --                     Automatic
R1#show ip dhcp server statistics
                ^
% Invalid input detected at '^' marker.
	
```
