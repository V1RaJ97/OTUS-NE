```
Building configuration...

Current configuration : 1418 bytes
!
! Last configuration change at 11:49:34 UTC Mon May 5 2025
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R21
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
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
no ip domain lookup
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
!
!
!
!
!
redundancy
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
!
!
interface Loopback1
 ip address 20.20.15.21 255.255.255.255
!
interface Ethernet0/0
 ip address 20.20.15.1 255.255.255.252
!
interface Ethernet0/1
 ip address 30.30.22.2 255.255.255.252
!
interface Ethernet0/2
 ip address 40.40.21.2 255.255.255.252
!
interface Ethernet0/3
 no ip address
 shutdown
!
interface Ethernet1/0
 no ip address
 shutdown
!
interface Ethernet1/1
 no ip address
 shutdown
!
interface Ethernet1/2
 no ip address
 shutdown
!
interface Ethernet1/3
 no ip address
 shutdown
!
router bgp 301
 bgp log-neighbor-changes
 network 20.20.0.0 mask 255.255.0.0
 neighbor 20.20.15.2 remote-as 1001
 neighbor 30.30.22.1 remote-as 101
 neighbor 40.40.21.1 remote-as 520
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 20.20.0.0 255.255.0.0 Null0
!
!
!
!
control-plane
!
!
!
!
!
!
!
!
line con 0
 logging synchronous
line aux 0
line vty 0 4
 login
 transport input none
!
!
end

```
