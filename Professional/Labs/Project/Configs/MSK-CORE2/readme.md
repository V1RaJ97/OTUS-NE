```
Building configuration...

Current configuration : 2334 bytes
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname MSK-CORE2
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
crypto isakmp policy 10
 encr aes 256
 hash sha256
 authentication pre-share
 group 14
crypto isakmp key Cisco123 address 0.0.0.0
!
!
crypto ipsec transform-set TS-SET esp-aes 256 esp-sha256-hmac
 mode transport
!
crypto ipsec profile DMVPN-PROFILE
 set transform-set TS-SET
!
!
!
!
!
!
!
interface Loopback1
 ip address 10.10.15.22 255.255.255.255
 ip ospf 1 area 10
!
interface Tunnel0
 ip address 10.1.1.2 255.255.255.0
 no ip redirects
 ip nhrp authentication dmvpn
 ip nhrp map multicast dynamic
 ip nhrp network-id 100
 ip nhrp redirect
 ip ospf network point-to-multipoint
 ip ospf 1 area 0
 tunnel source Ethernet0/1
 tunnel mode gre multipoint
 tunnel key 100
 tunnel protection ipsec profile DMVPN-PROFILE
!
interface Ethernet0/0
 ip address 10.10.22.1 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
 ip ospf network point-to-point
 ip ospf 1 area 10
!
interface Ethernet0/1
 ip address 11.12.10.2 255.255.255.252
 ip nat outside
 ip virtual-reassembly in
!
interface Ethernet0/2
 ip address 10.10.21.1 255.255.255.252
 ip nat inside
 ip virtual-reassembly in
 ip ospf network point-to-point
 ip ospf 1 area 10
!
interface Ethernet0/3
 no ip address
 shutdown
!
router ospf 1
 default-information originate
!
router bgp 1001
 bgp log-neighbor-changes
 neighbor 10.10.15.21 remote-as 1001
 neighbor 10.10.15.21 update-source Loopback1
 neighbor 11.12.10.1 remote-as 102
 neighbor 11.12.10.1 route-map BLOCK-OUT out
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
ip nat inside source list NAT-INSIDE interface Ethernet0/1 overload
!
ip access-list standard NAT-INSIDE
 permit 10.10.0.0 0.0.255.255
!
!
ip prefix-list BLOCK-ALL seq 5 deny 0.0.0.0/0 le 32
!
route-map BLOCK-OUT deny 10
 match ip address prefix-list BLOCK-ALL
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
