# Лабораторная работа - Маршрутизация на основе политик (PBR) 
## Cхема сети
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/b8e876b8227129f38d1e4bd51fe22bc380d0ac2b/Professional/Labs/PBR/Wireshark.png)
## Цели
```
1. Настроить политику маршрутизации в офисе Чокурдах
2. Распределить трафик между 2 линками
```
## Описание
```
Настроите политику маршрутизации для сетей офиса.
Распределите трафик между двумя линками с провайдером.
Настроите отслеживание линка через технологию IP SLA.(только для IPv4)
Настройте для офиса Лабытнанги маршрут по-умолчанию.
План работы и изменения зафиксированы в документации .
```
### Маршрут по умолчанию
```
Прописываем маршрут по умолчанию на R27:
R27(config)#ip route 0.0.0.0 0.0.0.0 40.40.27.1
```
```
R27#sh ip route
Gateway of last resort is 40.40.27.1 to network 0.0.0.0

S*    0.0.0.0/0 [1/0] via 40.40.27.1
      10.0.0.0/32 is subnetted, 1 subnets
C        10.21.0.27 is directly connected, Loopback1
      40.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C        40.40.27.0/30 is directly connected, Ethernet0/0
L        40.40.27.2/32 is directly connected, Ethernet0/0
```
### Настройка PBR
```
Cоздаем access-list на R28:
R28(config)#ip access-list extended VPC30
R28(config-ext-nacl)#permit ip host 10.22.131.2 any
R28(config)#ip access-list extended VPC31
R28(config-ext-nacl)#permit ip host 10.22.132.2 any
```
```
R28(config)#route-map TRIADA1 permit 10
R28(config-route-map)#match ip address ACL-VPC30
R28(config-route-map)#set ip next
R28(config-route-map)#set ip next-hop 40.40.29.1
R28(config-route-map)#exit
R28(config)#route-map TRIADA2 permit 20
R28(config-route-map)#match ip address ACL-VPC31
R28(config-route-map)#set ip next-hop 40.40.28.1
```
```
R28#sh route-map
route-map TRIADA2, permit, sequence 20
  Match clauses:
    ip address (access-lists): ACL-VPC31
  Set clauses:
    ip next-hop 40.40.28.1
  Policy routing matches: 0 packets, 0 bytes
route-map TRIADA1, permit, sequence 10
  Match clauses:
    ip address (access-lists): ACL-VPC30
  Set clauses:
    ip next-hop 40.40.29.1
  Policy routing matches: 0 packets, 0 bytes
```
### Настройка IP SLA
```
R28(config)#ip sla 1
R28(config-ip-sla)#icmp-echo 40.40.29.1 source-ip 40.40.29.2
R28(config-ip-sla-echo)#frequency 5
R28(config-ip-sla-echo)#exit
R28(config)#ip sla schedule 1 life forever start-time now
R28(config)#ip sla 2
R28(config-ip-sla)#icmp-echo 40.40.28.1 source-ip 40.40.28.2
R28(config-ip-sla-echo)#frequency 5
R28(config-ip-sla-echo)#exit
R28(config)#ip sla schedule 2 life forever start-time now
```
Проверка:
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/b8e876b8227129f38d1e4bd51fe22bc380d0ac2b/Professional/Labs/PBR/Wireshark.png)

```
Статистика:
R28#sh ip sla statistics
IPSLAs Latest Operation Statistics
IPSLA operation id: 1
        Latest RTT: 1 milliseconds
Latest operation start time: 16:05:51 UTC Fri Mar 7 2025
Latest operation return code: OK
Number of successes: 239
Number of failures: 0
Operation time to live: Forever
!
IPSLA operation id: 2
        Latest RTT: 1 milliseconds
Latest operation start time: 16:05:52 UTC Fri Mar 7 2025
Latest operation return code: OK
Number of successes: 215
Number of failures: 1
Operation time to live: Forever
```
### Настройка автоподключения и проверки доступности провайдеров
```
R28(config)#route-map TRIADA1 permit 10
R28(config-route-map)#match ip address ACL-VPC30
R28(config-route-map)#set ip next-hop verify-availability 40.40.29.1 10 track 1
R28(config-route-map)#set ip next-hop verify-availability 40.40.28.1 20 track 2
R28(config-route-map)#set ip next-hop 40.40.29.1
R28(config)#route-map TRIADA2 permit 20
R28(config-route-map)#match ip address ACL-VPC31
R28(config-route-map)#set ip nex-hop verify-availability 40.40.28.1 10 track 2
R28(config-route-map)#set ip nex-hop verify-availability 40.40.29.1 20 track 1
R28(config-route-map)#set ip next-hop 40.40.28.1
R28(config-route-map)#exit

R28(config)#track 1 ip sla 1 reachability
R28(config)#track 2 ip sla 2 reachability
```
```
R28#sh track brief
Track Type        Instance                   Parameter        State Last Change
1     ip sla      1                          reachability     Up    00:01:36
2     ip sla      2                          reachability     Up    00:01:29
```
### Настройка статических маршрутов на маршрутизаторах R25, R26 и R27 для проверки.
```
R28(config)#ip route 10.21.0.27 255.255.255.255 40.40.28.1 name v_labintagi
R28(config)#ip route 10.21.0.27 255.255.255.255 40.40.29.1 name v_labintagi
```
```
R25(config)#ip route 10.22.0.0 255.255.0.0 40.40.28.2 name track_test
R25(config)#ip route 10.22.0.0 255.255.0.0 40.40.26.2 150 name v_r26
R25(config)#ip route 10.21.0.27 255.255.255.255 40.40.27.2 name v_labintagi
```
```
R26(config)#ip route 10.22.0.0 255.255.0.0 40.40.29.2 name v_chokh
R26(config)#ip route 10.21.0.27 255.255.255.255 40.40.26.1 name v_labintagi
```
