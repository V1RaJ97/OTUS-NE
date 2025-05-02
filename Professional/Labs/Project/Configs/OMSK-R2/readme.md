```
Building configuration...

Current configuration : 1922 bytes
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname OMSK-R2
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
ip dhcp excluded-address 10.30.111.1 10.30.111.20
ip dhcp excluded-address 10.30.112.1 10.30.112.20
!
ip dhcp pool R2_SBER_VLAN
 network 10.30.111.0 255.255.255.0
 default-router 10.30.111.3
 lease 7
!
ip dhcp pool R2_ALFA_VLAN
 network 10.30.112.0 255.255.255.0
 default-router 10.30.112.3
 lease 7
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
 ip address 10.30.15.12 255.255.255.255
 ip ospf 1 area 30
!
interface Ethernet0/0
 ip address 10.30.12.2 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 30
!
interface Ethernet0/1
 no ip address
!
interface Ethernet0/1.30
 encapsulation dot1Q 30
 ip address 10.30.30.2 255.255.255.0
 standby version 2
 standby 30 ip 10.30.30.3
!
interface Ethernet0/1.111
 encapsulation dot1Q 111
 ip address 10.30.111.2 255.255.255.0
 standby version 2
 standby 111 ip 10.30.111.3
!
interface Ethernet0/1.112
 encapsulation dot1Q 112
 ip address 10.30.112.2 255.255.255.0
 standby version 2
 standby 112 ip 10.30.112.3
!
interface Ethernet0/2
 ip address 10.30.21.2 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 30
!
interface Ethernet0/3
 no ip address
 shutdown
!
router ospf 1
 network 10.30.30.0 0.0.0.255 area 30
 network 10.30.111.0 0.0.0.255 area 30
 network 10.30.112.0 0.0.0.255 area 30
 network 10.30.113.0 0.0.0.255 area 30
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
