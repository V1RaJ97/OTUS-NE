# Лабраторная работа - Настройка DHCPv6 
## Топология 
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/b6a7aefbaeb1e566dd24ac2d593b55b29fb85bb6/Professional/Labs/DHCP/IPV6/%D0%A2%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F(ipv6).png)
## Таблица адресации
|  Устройство  |  Интерфейс  |           IPv6-адрес            |
|:------------:|:-----------:|:-------------------------------:|
|      R1      |    G0/0/0   | 2001:db8:acad:2::1/64 (fe80::1) |
|      R1      |    G0/0/1   | 2001:db8:acad:1::1/64 (fe80::1) |
|      R2      |    G0/0/0   | 2001:db8:acad:2::2/64 (fe80::2) |
|      R2      |    G0/0/1   | 2001:db8:acad:3::1/64 (fe80::1) |
|     PC-A     |     NIC     |              DHCP               |
|     PC-B     |     NIC     |              DHCP               |

## Задачи
1. Создание сети и настройка основных параметров устройства
2. Проверка назначения адреса SLAAC от R1
3. Настройка и проверка сервера DHCPv6 без гражданства на R1
4. Настройка и проверка состояния DHCPv6 сервера на R1
5. Настройка и проверка DHCPv6 Relay на R2

## Часть 1. Создание сети и настройка основных параметров устройства
### Настройка коммутаоров S1 и S2
```
Switch>en
Switch#conf ter
Switch(config)#hostname S1
S1(config)#no ip domain-lookup
S1(config)#service password-encryption 
S1(config)#enable secret class
S1(config)#banner motd 1 Unauthorized access is strictly prohibited 1
```
```
S1(config)#line con 0
S1(config-line)#logging synchronous 
S1(config-line)#password cisco
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
S1(config)#interface range fa0/1-4
S1(config-if-range)#shutdown 
S1(config-if-range)#exit
S1(config)#interface range fa0/7-24
S1(config-if-range)#shutdown 
S1(config-if-range)#exit
S1(config)#interface range gi0/1-2
S1(config-if-range)#shutdown 
S1(config-if-range)#end
S1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```

#### Финальная конфигурация коммутатора S2
```
S2#show running-config 
Building configuration...

Current configuration : 1550 bytes
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
 shutdown
!
interface FastEthernet0/2
 shutdown
!
interface FastEthernet0/3
 shutdown
!
interface FastEthernet0/4
 shutdown
!
interface FastEthernet0/5
!
interface FastEthernet0/6
 shutdown
!
interface FastEthernet0/7
 shutdown
!
interface FastEthernet0/8
 shutdown
!
interface FastEthernet0/9
 shutdown
!
interface FastEthernet0/10
 shutdown
!
interface FastEthernet0/11
 shutdown
!
interface FastEthernet0/12
 shutdown
!
interface FastEthernet0/13
 shutdown
!
interface FastEthernet0/14
 shutdown
!
interface FastEthernet0/15
 shutdown
!
interface FastEthernet0/16
 shutdown
!
interface FastEthernet0/17
 shutdown
!
interface FastEthernet0/18
!
interface FastEthernet0/19
 shutdown
!
interface FastEthernet0/20
 shutdown
!
interface FastEthernet0/21
 shutdown
!
interface FastEthernet0/22
 shutdown
!
interface FastEthernet0/23
 shutdown
!
interface FastEthernet0/24
 shutdown
!
interface GigabitEthernet0/1
 shutdown
!
interface GigabitEthernet0/2
 shutdown
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
 logging synchronous
 login
line vty 5 15
 login
!
end
```
### Настройка маршрутизаторов R1 и R2
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
```
```
R1(config)#ipv6 unicast-routing 
R1(config)#exit
R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
```
Проводим аналогичные натройки на R2
```
#### Настройка интерфейсов и маршрутизации на R1 и R2
```
R1(config)#int g0/0/0
R1(config-if)#ipv6 enable 
R1(config-if)#ipv6 address 2001:db8:acad:2::1/64
R1(config-if)#ipv6 address fe80::1 link-local
R1(config-if)#no shutdown
R1(config-if)#exit
R1(config)#int g0/0/1
R1(config-if)#ipv6 enable 
R1(config-if)#ipv6 address 2001:db8:acad:1::1/64
R1(config-if)#ipv6 address fe80::1 link-local 
R1(config-if)#no shutdown 
R1(config-if)#exit
```
```
Аналогичныйм образом настраиваем интерфейсы на R2 исходя из таблицы маршрутизации.
```
```
R1(config)#ipv6 route ::/0 2001:db8:acad:2::2
R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
```
R2(config)#ipv6 route ::/0 2001:db8:acad:2::1
R2#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
##### Пинг интерфейса G0/0/1 R1 c маршрутизатора R2
```
R2#ping 2001:db8:acad:1::1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:db8:acad:1::1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms
```
## Часть 2. Проверка назначения адреса SLAAC от R1
IPconfig на PC-A:
```
C:\>ipconfig

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Link-local IPv6 Address.........: FE80::2D0:58FF:FE14:9C29
   IPv6 Address....................: 2001:DB8:ACAD:1:2D0:58FF:FE14:9C29
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
```
## Часть 3. Настройка и проверка сервера DHCPv6 на R1
```
C:\>ipconfig /all

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Physical Address................: 00D0.5814.9C29
   Link-local IPv6 Address.........: FE80::2D0:58FF:FE14:9C29
   IPv6 Address....................: 2001:DB8:ACAD:1:2D0:58FF:FE14:9C29
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
   DHCP Servers....................: 0.0.0.0
   DHCPv6 IAID.....................: 
   DHCPv6 Client DUID..............: 00-01-00-01-90-3C-70-8D-00-D0-58-14-9C-29
   DNS Servers.....................: ::
                                     0.0.0.0
```
### Настройка R1 для предоставления DHCPv6 
```
R1(config)#ipv6 dhcp pool R1-STATELESS
R1(config-dhcpv6)#dns-server 2001:db8:acad::254
R1(config-dhcpv6)#domain-name STATELESS.com
```
```
R1(config)#interface g0/0/1
R1(config-if)#ipv6 nd other-config-flag 
R1(config-if)#ipv6 dhcp server R1-STATELESS
R1(config-if)#end
R1#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```
IPconfig на PC-A
```
C:\>ipconfig /all

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: STATELESS.com 
   Physical Address................: 00D0.5814.9C29
   Link-local IPv6 Address.........: FE80::2D0:58FF:FE14:9C29
   IPv6 Address....................: 2001:DB8:ACAD:1:2D0:58FF:FE14:9C29
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
   DHCP Servers....................: 0.0.0.0
   DHCPv6 IAID.....................: 520893310
   DHCPv6 Client DUID..............: 00-01-00-01-90-3C-70-8D-00-D0-58-14-9C-29
   DNS Servers.....................: 2001:DB8:ACAD::254
                                     0.0.0.0
```
Пинг интерфейса g0/0/1 на R2
```
C:\>ping 2001:db8:acad:3::1

Pinging 2001:db8:acad:3::1 with 32 bytes of data:

Reply from 2001:DB8:ACAD:3::1: bytes=32 time=1ms TTL=254
Reply from 2001:DB8:ACAD:3::1: bytes=32 time<1ms TTL=254
Reply from 2001:DB8:ACAD:3::1: bytes=32 time=1ms TTL=254
Reply from 2001:DB8:ACAD:3::1: bytes=32 time<1ms TTL=254

Ping statistics for 2001:DB8:ACAD:3::1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 1ms, Average = 0ms
```
## Часть 4. Настройка сервера DHCPv6 с сохранением состояния на R1
```
R1(config)#ipv6 dhcp pool R2-STATEFUL
R1(config-dhcpv6)#address prefix 2001:db8:acad:3:aaa::/80
R1(config-dhcpv6)#dns-server 2001:db8:acad::254
R1(config-dhcpv6)#domain-name STATEFUL.com
R1(config-dhcpv6)#exit
R1(config)#interface g0/0/0
R1(config-if)#ipv6 dhcp server R2-STATEFUL
R1(config-if)#end
```
## Часть 5. Настройка и проверка ретрансляции DHCPv6 на R2.
IPconfig PC-B
```
C:\>ipconfig /all

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Physical Address................: 0004.9A3E.B2E0
   Link-local IPv6 Address.........: FE80::204:9AFF:FE3E:B2E0
   IPv6 Address....................: 2001:DB8:ACAD:3:204:9AFF:FE3E:B2E0
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: FE80::1
                                     0.0.0.0
   DHCP Servers....................: 0.0.0.0
   DHCPv6 IAID.....................: 
   DHCPv6 Client DUID..............: 00-01-00-01-10-D3-01-19-00-04-9A-3E-B2-E0
   DNS Servers.....................: ::
                                     0.0.0.0
```
### Настройка R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/0/1
```
R2(config)#int g0/0/1
R2(config-if)#ipv6 nd managed-config-flag 
R2(config-if)#ipv6 dhcp relay destination 2001:db8:acad:2::1 g0/0/0
R2#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```

