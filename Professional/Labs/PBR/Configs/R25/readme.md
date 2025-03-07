Building configuration...

Current configuration : 1389 bytes
!
! Last configuration change at 17:10:16 UTC Fri Mar 7 2025
!
version 15.4
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R25
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
 ip address 40.40.15.25 255.255.255.255
!
interface Ethernet0/0
 ip address 40.40.25.2 255.255.255.252
!
interface Ethernet0/1
 ip address 40.40.27.1 255.255.255.252
!
interface Ethernet0/2
 ip address 40.40.26.1 255.255.255.252
!
interface Ethernet0/3
 ip address 40.40.28.1 255.255.255.252
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
ip route 10.21.0.27 255.255.255.255 40.40.27.2 name v_labintagi
ip route 10.22.0.0 255.255.0.0 40.40.28.2 name track_test
ip route 10.22.0.0 255.255.0.0 40.40.26.2 150 name v_r26
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

