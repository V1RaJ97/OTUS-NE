```
Building configuration...

Current configuration : 1423 bytes
!
! Last configuration change at 15:30:45 UTC Sat Mar 22 2025
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R26
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
 ip address 40.40.15.26 255.255.255.255
 ip router isis
!
interface Ethernet0/0
 ip address 40.40.20.2 255.255.255.252
 ip router isis
!
interface Ethernet0/1
 ip address 40.40.29.1 255.255.255.252
!
interface Ethernet0/2
 ip address 40.40.26.2 255.255.255.252
 ip router isis
!
interface Ethernet0/3
 ip address 40.40.19.1 255.255.255.252
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
 net 49.2604.0040.0150.2600
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 10.21.0.27 255.255.255.255 40.40.26.1 name v_labintagi
ip route 10.22.0.0 255.255.0.0 40.40.29.2 name v_chokh
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
