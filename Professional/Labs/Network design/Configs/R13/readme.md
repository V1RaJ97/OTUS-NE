```
Current configuration : 1530 bytes
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R13
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
 ip address 10.10.15.13 255.255.255.255
!
interface Ethernet0/0
 no ip address
!
interface Ethernet0/0.30
 encapsulation dot1Q 30
 ip address 10.10.30.2 255.255.255.0
 standby version 2
 standby 30 ip 10.10.30.3
!
interface Ethernet0/0.111
 encapsulation dot1Q 111
 ip address 10.10.111.2 255.255.255.0
 standby version 2
 standby 111 ip 10.10.111.3
!
interface Ethernet0/0.112
 encapsulation dot1Q 112
 ip address 10.10.112.2 255.255.255.0
 standby version 2
 standby 112 ip 10.10.112.3
!
interface Ethernet0/1
 no ip address
 shutdown
!
interface Ethernet0/2
 ip address 10.10.13.2 255.255.255.252
!
interface Ethernet0/3
 ip address 10.10.14.2 255.255.255.252
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
