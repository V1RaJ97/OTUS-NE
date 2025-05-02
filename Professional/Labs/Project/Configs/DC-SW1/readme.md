```
Building configuration...

Current configuration : 1080 bytes
!
! Last configuration change at 11:44:43 UTC Fri May 2 2025
!
version 15.2
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
service compress-config
!
hostname DC-SW1
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
 switchport trunk allowed vlan 30,100
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 30
 switchport mode trunk
!
interface Ethernet0/1
 switchport access vlan 100
 switchport mode access
!
interface Ethernet0/2
 switchport access vlan 100
 switchport mode access
!
interface Ethernet0/3
 switchport access vlan 100
 switchport mode access
!
interface Vlan30
 ip address 10.50.30.11 255.255.255.0
!
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
