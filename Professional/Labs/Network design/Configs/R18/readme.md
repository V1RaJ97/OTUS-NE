```

Current configuration : 953 bytes
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R18
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
 ip address 10.20.15.18 255.255.255.255
!
interface Ethernet0/0
 ip address 10.20.16.1 255.255.255.252
!
interface Ethernet0/1
 ip address 10.20.17.1 255.255.255.252
!
interface Ethernet0/2
 ip address 40.40.18.2 255.255.255.252
!
interface Ethernet0/3
 ip address 40.40.19.2 255.255.255.252
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
