```
Building configuration...

Current configuration : 2286 bytes
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname MSK-R2
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
ip dhcp excluded-address 10.10.111.1 10.10.111.20
ip dhcp excluded-address 10.10.112.1 10.10.112.20
ip dhcp excluded-address 10.10.113.1 10.10.113.20
!
ip dhcp pool R2_SBER_VLAN
 network 10.10.111.0 255.255.255.0
 default-router 10.10.111.3
 lease 7
!
ip dhcp pool R2_ALFA_VLAN
 network 10.10.112.0 255.255.255.0
 default-router 10.10.112.3
 lease 7
!
ip dhcp pool R2_VTB_VLAN
 network 10.10.113.0 255.255.255.0
 default-router 10.10.113.3
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
 ip address 10.10.15.12 255.255.255.255
 ip ospf 1 area 10
!
interface Ethernet0/0
 ip address 10.10.22.2 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 10
!
interface Ethernet0/1
 no ip address
!
interface Ethernet0/1.30
 description Management
 encapsulation dot1Q 30
 ip address 10.10.30.2 255.255.255.0
 standby version 2
 standby 30 ip 10.10.30.3
!
interface Ethernet0/1.111
 description SBER
 encapsulation dot1Q 111
 ip address 10.10.111.2 255.255.255.0
 standby version 2
 standby 111 ip 10.10.111.3
!
interface Ethernet0/1.112
 description ALFA
 encapsulation dot1Q 112
 ip address 10.10.112.2 255.255.255.0
 standby version 2
 standby 112 ip 10.10.112.3
!
interface Ethernet0/1.113
 description VTB
 encapsulation dot1Q 113
 ip address 10.10.113.2 255.255.255.0
 standby version 2
 standby 113 ip 10.10.113.3
!
interface Ethernet0/2
 ip address 10.10.12.2 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 10
!
interface Ethernet0/3
 no ip address
 shutdown
!
router ospf 1
 network 10.10.30.0 0.0.0.255 area 10
 network 10.10.111.0 0.0.0.255 area 10
 network 10.10.112.0 0.0.0.255 area 10
 network 10.10.113.0 0.0.0.255 area 10
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
