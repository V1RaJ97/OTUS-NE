```
Current configuration : 1308 bytes
!
! Last configuration change at 22:57:37 UTC Sun Feb 23 2025
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname SW3
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
!
!
!
!
!
no ip domain-lookup
ip cef
no ipv6 cef
!
!
!
spanning-tree mode rapid-pvst
spanning-tree extend system-id
!
vlan internal allocation policy ascending
!
!
!
!
!
!
!
!
!
!
!
!
!
interface Ethernet0/0
 switchport trunk allowed vlan 30,111,112
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 30
 switchport mode trunk
!
interface Ethernet0/1
 switchport trunk allowed vlan 30,111,112
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 30
 switchport mode trunk
!
interface Ethernet0/2
 switchport access vlan 111
 switchport mode access
!
interface Ethernet0/3
 shutdown
!
interface Ethernet1/0
 shutdown
!
interface Ethernet1/1
 shutdown
!
interface Ethernet1/2
 shutdown
!
interface Ethernet1/3
 shutdown
!
interface Vlan30
 ip address 10.10.30.13 255.255.255.0
!
ip default-gateway 10.10.30.1
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
!
!
!
!
!
control-plane
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
!
!
end
```
