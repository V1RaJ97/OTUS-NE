```
Building configuration...

Current configuration : 1847 bytes
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname GN-R2
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
 ip address 11.40.15.12 255.255.255.255
!
interface Ethernet0/0
 ip address 11.40.12.2 255.255.255.252
!
interface Ethernet0/1
 ip address 11.40.14.1 255.255.255.252
!
interface Ethernet0/2
 ip address 11.40.112.1 255.255.255.252
!
interface Ethernet0/3
 no ip address
!
interface Ethernet1/0
 ip address 11.40.16.1 255.255.255.252
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
interface Ethernet2/0
 no ip address
 shutdown
!
interface Ethernet2/1
 no ip address
 shutdown
!
interface Ethernet2/2
 no ip address
 shutdown
!
interface Ethernet2/3
 no ip address
 shutdown
!
interface Ethernet3/0
 no ip address
 shutdown
!
interface Ethernet3/1
 no ip address
 shutdown
!
interface Ethernet3/2
 no ip address
 shutdown
!
interface Ethernet3/3
 no ip address
 shutdown
!
router bgp 401
 bgp log-neighbor-changes
 redistribute connected
 neighbor 11.40.12.1 remote-as 401
 neighbor 11.40.12.1 next-hop-self
 neighbor 11.40.14.2 remote-as 401
 neighbor 11.40.14.2 next-hop-self
 neighbor 11.40.16.2 remote-as 401
 neighbor 11.40.16.2 next-hop-self
 neighbor 11.40.112.2 remote-as 202
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
