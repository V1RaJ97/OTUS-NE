```
Building configuration...

Current configuration : 1470 bytes
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R14
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
 ip address 10.10.15.14 255.255.255.255
 ip ospf 1 area 0
!
interface Ethernet0/0
 ip address 10.10.12.1 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
!
interface Ethernet0/1
 ip address 10.10.14.1 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
!
interface Ethernet0/2
 ip address 30.30.14.2 255.255.255.252
!
interface Ethernet0/3
 ip address 10.10.19.2 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 101
!
router ospf 1
 area 101 stub no-summary
 default-information originate
!
router bgp 1001
 bgp log-neighbor-changes
 network 10.10.15.0 mask 255.255.255.0
 network 10.10.111.0 mask 255.255.255.0
 network 10.10.112.0 mask 255.255.255.0
 neighbor 30.30.14.1 remote-as 101
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 30.30.14.1
ip route 10.10.15.0 255.255.255.0 Null0
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
