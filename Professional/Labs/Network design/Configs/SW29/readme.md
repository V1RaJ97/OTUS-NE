```
Current configuration : 1041 bytes
!
! Last configuration change at 23:12:44 UTC Sun Feb 23 2025
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname SW29
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
ip cef
no ipv6 cef
!
!
spanning-tree mode pvst
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
 switchport access vlan 131
 switchport mode access
!
interface Ethernet0/1
 switchport access vlan 132
 switchport mode access
!
interface Ethernet0/2
 switchport trunk allowed vlan 30,131,132
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 30
 switchport mode trunk
!
interface Ethernet0/3
!
interface Vlan30
 ip address 10.22.30.29 255.255.255.0
!
ip default-gateway 10.22.30.1
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
