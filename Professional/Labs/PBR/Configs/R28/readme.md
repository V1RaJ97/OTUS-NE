```
Building configuration...

Current configuration : 2448 bytes
!
! Last configuration change at 14:03:34 UTC Wed Mar 12 2025
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
track 1 ip sla 1 reachability
!
track 2 ip sla 2 reachability
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
 ip policy route-map TRIADA1
!
interface Ethernet0/2.132
 encapsulation dot1Q 132
 ip address 10.22.132.1 255.255.255.0
 ip policy route-map TRIADA2
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
ip route 10.21.0.27 255.255.255.255 40.40.28.1 name v_labintagi
ip route 10.21.0.27 255.255.255.255 40.40.29.1 name v_labintagi
!
ip access-list extended VPC30
 permit ip host 10.22.131.2 any
ip access-list extended VPC31
 permit ip host 10.22.132.2 any
!
ip sla 1
 icmp-echo 40.40.29.1 source-ip 40.40.29.2
 frequency 5
ip sla schedule 1 life forever start-time now
ip sla 2
 icmp-echo 40.40.28.1 source-ip 40.40.28.2
 frequency 5
ip sla schedule 2 life forever start-time now
!
route-map TRIADA2 permit 20
 match ip address ACL-VPC31
 set ip next-hop verify-availability 40.40.28.1 10 track 2
 set ip next-hop verify-availability 40.40.29.1 20 track 1
 set ip next-hop 40.40.28.1
!
route-map TRIADA1 permit 10
 match ip address ACL-VPC30
 set ip next-hop verify-availability 40.40.29.1 10 track 1
 set ip next-hop verify-availability 40.40.28.1 20 track 2
 set ip next-hop 40.40.29.1
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
