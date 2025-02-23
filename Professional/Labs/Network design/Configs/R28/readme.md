```
Current configuration : 1380 bytes
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R28
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
 ip address 10.22.0.28 255.255.255.255
!
interface Ethernet0/0
 ip address 40.40.29.2 255.255.255.252
!
interface Ethernet0/1
 ip address 40.40.28.2 255.255.255.252
!
interface Ethernet0/2
 no ip address
!
interface Ethernet0/2.30
 encapsulation dot1Q 30
 ip address 10.22.30.1 255.255.255.0
!
interface Ethernet0/2.131
 encapsulation dot1Q 131
 ip address 10.22.131.1 255.255.255.0
!
interface Ethernet0/2.132
 encapsulation dot1Q 132
 ip address 10.22.132.1 255.255.255.0
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
