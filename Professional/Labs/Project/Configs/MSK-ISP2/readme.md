```
Building configuration...

Current configuration : 1144 bytes
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname MSK-ISP2
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
 ip address 11.12.15.12 255.255.255.255
!
interface Ethernet0/0
 ip address 11.12.10.1 255.255.255.252
!
interface Ethernet0/1
 ip address 11.40.114.2 255.255.255.252
!
interface Ethernet0/2
 no ip address
 shutdown
!
interface Ethernet0/3
 no ip address
 shutdown
!
router bgp 102
 bgp log-neighbor-changes
 redistribute connected
 neighbor 11.12.10.2 remote-as 1001
 neighbor 11.12.10.2 next-hop-self
 neighbor 11.12.10.2 default-originate
 neighbor 11.40.114.1 remote-as 401
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
