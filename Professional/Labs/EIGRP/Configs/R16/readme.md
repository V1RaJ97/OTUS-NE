```
Building configuration...

Current configuration : 1986 bytes
!
! Last configuration change at 15:53:05 UTC Sat Mar 29 2025
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R16
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
 ip address 10.20.15.16 255.255.255.255
!
interface Ethernet0/0
 no ip address
!
interface Ethernet0/0.30
 encapsulation dot1Q 30
 ip address 10.20.30.2 255.255.255.0
 standby version 2
 standby 30 ip 10.20.30.3
!
interface Ethernet0/0.121
 encapsulation dot1Q 121
 ip address 10.20.121.2 255.255.255.0
 standby version 2
 standby 121 ip 10.20.121.3
!
interface Ethernet0/0.122
 encapsulation dot1Q 122
 ip address 10.20.122.2 255.255.255.0
 standby version 2
 standby 122 ip 10.20.122.3
!
interface Ethernet0/1
 ip address 10.20.16.2 255.255.255.252
!
interface Ethernet0/2
 no ip address
 shutdown
!
interface Ethernet0/3
 ip address 10.20.32.1 255.255.255.252
!
!
router eigrp SPB-EIGRP
 !
 address-family ipv4 unicast autonomous-system 2042
  !
  af-interface Ethernet0/0.30
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/0.121
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/0.122
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address 10.20.0.0 255.255.0.0
  exit-af-interface
  !
  af-interface Ethernet0/3
   summary-address 0.0.0.0 0.0.0.0
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 10.20.0.0 0.0.255.255
  eigrp stub summary
 exit-address-family
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
