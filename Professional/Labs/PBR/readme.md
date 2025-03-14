# Лабораторная работа - Маршрутизация на основе политик (PBR) 
## Cхема сети
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/8f5f9e81ff030f5e81c5c911b7569e6a927bddeb/Professional/Labs/PBR/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0%20%D1%81%D0%B5%D1%82%D0%B8.png)
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

```
Прописываем route-map на саб-интерфейсы:
interface Ethernet0/2.131
 encapsulation dot1Q 131
 ip address 10.22.131.1 255.255.255.0
 ip policy route-map TRIADA1
!
interface Ethernet0/2.132
 encapsulation dot1Q 132
 ip address 10.22.132.1 255.255.255.0
 ip policy route-map TRIADA2
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
### Проверка
```
Все линки доступны:
VPC30> trace 10.21.0.27
trace to 10.21.0.27, 8 hops max, press Ctrl+C to stop
 1   10.22.131.1   0.877 ms  0.349 ms  0.104 ms
 2   40.40.29.1   0.375 ms  0.278 ms  0.211 ms
 3   40.40.26.1   0.388 ms  0.409 ms  0.325 ms
 4   *40.40.27.2   0.662 ms (ICMP type:3, code:3, Destination port unreachable)

VPC31> trace 10.21.0.27
trace to 10.21.0.27, 8 hops max, press Ctrl+C to stop
 1   10.22.132.1   0.420 ms  0.218 ms  0.221 ms
 2   40.40.28.1   0.637 ms  0.323 ms  0.436 ms
 3   *40.40.27.2   1.199 ms (ICMP type:3, code:3, Destination port unreachable)
```

```
Гасим e0/3 на R25:
VPC30> trace 10.21.0.27
trace to 10.21.0.27, 8 hops max, press Ctrl+C to stop
 1   10.22.131.1   1.187 ms  0.644 ms  0.856 ms
 2   40.40.29.1   1.056 ms  0.535 ms  0.935 ms
 3   40.40.26.1   2.058 ms  0.977 ms  0.914 ms
 4   *40.40.27.2   1.274 ms (ICMP type:3, code:3, Destination port unreachable)

VPC30> trace 10.21.0.27
trace to 10.21.0.27, 8 hops max, press Ctrl+C to stop
 1   10.22.132.1   0.411 ms  0.249 ms  0.331 ms
 2   40.40.29.1   0.417 ms  0.371 ms  0.357 ms
 3   40.40.26.1   0.539 ms  0.459 ms  0.477 ms
 4   *40.40.27.2   0.752 ms (ICMP type:3, code:3, Destination port unreachable)

```
### Конфиги [тут](https://github.com/V1RaJ97/OTUS-NE/tree/c93fb10ae6171735b805c7a9bd2f91629675df36/Professional/Labs/PBR/Configs).
