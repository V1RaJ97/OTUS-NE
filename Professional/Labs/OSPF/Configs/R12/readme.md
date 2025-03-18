```
Building configuration...

Current configuration : 1955 bytes
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R12
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
 ip address 10.10.15.12 255.255.255.255
 ip ospf 1 area 10
!
interface Ethernet0/0
 no ip address
!
interface Ethernet0/0.30
 encapsulation dot1Q 30
 ip address 10.10.30.1 255.255.255.0
 standby version 2
 standby 30 ip 10.10.30.3
 standby 30 priority 150
 standby 30 preempt
 ip ospf 1 area 10
!
interface Ethernet0/0.111
 encapsulation dot1Q 111
 ip address 10.10.111.1 255.255.255.0
 standby version 2
 standby 111 ip 10.10.111.3
 standby 111 priority 150
 standby 111 preempt
 ip ospf 1 area 10
!
interface Ethernet0/0.112
 encapsulation dot1Q 112
 ip address 10.10.112.1 255.255.255.0
 standby version 2
 standby 112 ip 10.10.112.3
 standby 112 priority 150
 standby 112 preempt
 ip ospf 1 area 10
!
interface Ethernet0/1
 no ip address
 shutdown
!
interface Ethernet0/2
 ip address 10.10.12.2 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
!
interface Ethernet0/3
 ip address 10.10.27.2 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
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
router ospf 1
 passive-interface Ethernet0/0.30
 passive-interface Ethernet0/0.111
 passive-interface Ethernet0/0.112
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
