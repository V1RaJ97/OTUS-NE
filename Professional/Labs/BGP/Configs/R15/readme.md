```
Building configuration...

Current configuration : 1587 bytes
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R15
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
 ip address 10.10.15.15 255.255.255.255
 ip ospf 1 area 0
!
interface Ethernet0/0
 ip address 10.10.13.1 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
!
interface Ethernet0/1
 ip address 10.10.27.1 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 0
!
interface Ethernet0/2
 ip address 20.20.15.2 255.255.255.252
!
interface Ethernet0/3
 ip address 10.10.20.1 255.255.255.252
 ip ospf network point-to-point
 ip ospf 1 area 102
!
router ospf 1
 area 102 filter-list prefix area-102 in
 default-information originate
!
router bgp 1001
 bgp log-neighbor-changes
 network 10.10.15.0 mask 255.255.255.0
 network 10.10.111.0 mask 255.255.255.0
 network 10.10.112.0 mask 255.255.255.0
 neighbor 20.20.15.1 remote-as 301
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip route 0.0.0.0 0.0.0.0 20.20.15.1
ip route 10.10.15.0 255.255.255.0 Null0
!
!
ip prefix-list area-102 seq 5 deny 10.10.19.0/30
ip prefix-list area-102 seq 10 deny 10.10.15.19/32
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
