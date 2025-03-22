```
Building configuration...

Current configuration : 1326 bytes
!
! Last configuration change at 15:25:56 UTC Sat Mar 22 2025
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R24
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
 ip address 40.40.15.24 255.255.255.255
 ip router isis
!
interface Ethernet0/0
 ip address 40.40.21.1 255.255.255.252
!
interface Ethernet0/1
 ip address 40.40.20.1 255.255.255.252
 ip router isis
!
interface Ethernet0/2
 ip address 40.40.24.2 255.255.255.252
 ip router isis
!
interface Ethernet0/3
 ip address 40.40.18.1 255.255.255.252
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
router isis
 net 49.2404.0040.0150.2400
 is-type level-2-only
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
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
