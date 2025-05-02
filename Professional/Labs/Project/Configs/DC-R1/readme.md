```
Building configuration...

Current configuration : 2175 bytes
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname DC-R1
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
 ip address 10.50.15.11 255.255.255.255
 ip ospf 1 area 50
!
interface Tunnel0
 ip address 10.1.1.51 255.255.255.0
 no ip redirects
 ip nhrp authentication dmvpn
 ip nhrp map 10.1.1.1 11.11.10.2
 ip nhrp map multicast 11.11.10.2
 ip nhrp map 10.1.1.2 11.12.10.2
 ip nhrp map multicast 11.12.10.2
 ip nhrp network-id 100
 ip nhrp nhs 10.1.1.1
 ip nhrp nhs 10.1.1.2
 ip nhrp shortcut
 ip ospf network point-to-multipoint
 ip ospf 1 area 0
 tunnel source Ethernet0/1
 tunnel mode gre multipoint
 tunnel key 100
 tunnel protection ipsec profile DMVPN-PROFILE
!
interface Ethernet0/0
 no ip address
!
interface Ethernet0/0.30
 encapsulation dot1Q 30
 ip address 10.50.30.1 255.255.255.0
 ip ospf 1 area 50
!
interface Ethernet0/0.100
 encapsulation dot1Q 100
 ip address 10.50.100.1 255.255.255.0
 ip ospf 1 area 50
!
interface Ethernet0/1
 ip address 11.40.117.2 255.255.255.252
!
interface Ethernet0/2
 no ip address
!
interface Ethernet0/3
 no ip address
 shutdown
!
router ospf 1
!
router bgp 5001
 bgp log-neighbor-changes
 neighbor 11.40.117.1 remote-as 401
 neighbor 11.40.117.1 route-map BLOCK-OUT out
!
ip forward-protocol nd
!
!
no ip http server
no ip http secure-server
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
