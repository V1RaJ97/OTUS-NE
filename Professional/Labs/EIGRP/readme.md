# Лабораторная работа - EIGRP
## Цель:
1. Настроить EIGRP в С.-Петербург
2. Использовать named EIGRPP
## Описание
```
В офисе С.-Петербург настроить EIGRP.
R32 получает только маршрут по умолчанию.
R16-17 анонсируют только суммарные префиксы.
Использовать EIGRP named-mode для настройки сети.
```
## Топология
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/af7b51a6b1e8e6cacd5a9d06661747da9ab4716e/Professional/Labs/EIGRP/%D0%A2%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F.png)

|  Устройство  |  Интерфейс |   IP-адрес    |      Маска      |    Шлюз    |
|--------------|------------|---------------|-----------------|------------|
| R18          | Lo1        | 10.20.15.18   | 255.255.255.255 |            |
| R18          | e0/0       | 10.20.16.1    | 255.255.255.252 |            | 
| R18          | e0/1       | 10.20.17.1    | 255.255.255.252 |            |
| R18          | e0/2       | 40.40.18.2    | 255.255.255.252 |            |
| R18          | e0/3       | 40.40.19.2    | 255.255.255.252 |            |
| R17          | Lo1        | 10.20.15.17   | 255.255.255.255 |            |
| R17          | e0/1       | 10.20.17.2    | 255.255.255.252 |            |
| R17          | VLAN121    | 10.20.121.1   | 255.255.255.0   |            |
| R17          | VLAN122    | 10.20.122.1   | 255.255.255.0   |            |
| R17          | VLAN30     | 10.20.30.1    | 255.255.255.0   |            |
| R16          | Lo1        | 10.20.15.16   | 255.255.255.255 |            |
| R16          | e0/1       | 10.20.16.2    | 255.255.255.252 |            |
| R16          | e0/3       | 10.20.32.1    | 255.255.255.252 |            |
| R16          | VLAN121    | 10.20.121.2   | 255.255.255.0   |            |
| R16          | VLAN122    | 10.20.122.2   | 255.255.255.0   |            |
| R16          | VLAN30     | 10.20.30.2    | 255.255.255.0   |            |
| R32          | Lo1        | 10.20.15.32   | 255.255.255.255 |            |
| R32          | e0/0       | 10.20.32.2    | 255.255.255.252 |            |

## Выполнение
### Настройка R18
```
R18(config)#router eigrp SPB-EIGRP
R18(config-router)#address-family ipv4 unicast autonomous-system 2042
R18(config-router-af)#network 10.20.0.0 0.0.255.255
R18(config-router-af)#topology base
R18(config-router-af-topology)#redistribute static
```
```
Маршруты до R25 и R26:
R18(config)#ip route 0.0.0.0 0.0.0.0 40.40.19.1
R18(config)#ip route 0.0.0.0 0.0.0.0 40.40.18.1
```
### Настройка R17
```
R17(config)#router eigrp SPB-EIGRP
R17(config-router)#address-family ipv4 unicast autonomous-system 2042
R17(config-router-af)#network 10.20.0.0 0.0.255.255
R17(config-router-af)#eigrp stub summary
R17(config-router-af)#af-interface e0/0.30
R17(config-router-af-interface)#passive-interface
R17(config-router-af-interface)#af-interface e0/0.121
R17(config-router-af-interface)#passive-interface
R17(config-router-af-interface)#af-interface e0/0.122
R17(config-router-af-interface)#passive-interface
R17(config-router-af-interface)exit
R17(config-router-af)#af-interface e0/1
R17(config-router-af-interface)#summary-address 10.20.0.0/16
R17(config-router-af-interface)#end
```
#### Проверка
```
R18#sh ip route eigrp
Gateway of last resort is 40.40.19.1 to network 0.0.0.0
      10.0.0.0/8 is variably subnetted, 6 subnets, 3 masks
D        10.20.0.0/16 [90/1024640] via 10.20.17.2, 00:02:24, Ethernet0/1

```
```
R17#sh ip route eigrp
Gateway of last resort is 10.20.17.1 to network 0.0.0.0
D*EX  0.0.0.0/0 [170/1536000] via 10.20.17.1, 00:15:56, Ethernet0/1
      10.0.0.0/8 is variably subnetted, 12 subnets, 4 masks
D        10.20.0.0/16 is a summary, 00:03:03, Null0
D        10.20.15.18/32 [90/1024640] via 10.20.17.1, 00:15:56, Ethernet0/1
D        10.20.16.0/30 [90/1536000] via 10.20.17.1, 00:15:56, Ethernet0/1

```
### Настройка R16
```
R16(config)#router eigrp SPB-EIGRP
R16(config-router)#address-family ipv4 autonomous-system 2042
R16(config-router-af)#network 10.20.0.0 0.0.255.255
R16(config-router-af)#af-interface e0/0.30
R16(config-router-af-interface)#passive-interface
R16(config-router-af-interface)#af-interface e0/0.121
R16(config-router-af-interface)#passive-interface
R16(config-router-af-interface)#af-interface e0/0.122
R16(config-router-af-interface)#passive-interface
R16(config-router-af-interface)#af-interface e0/1
R16(config-router-af-interface)#summary-address 10.20.0.0/16
R16(config-router-af-interface)#exit
R16(config-router-af)#eigrp stub summary
R16(config-router-af)#af-interface e0/3
R16(config-router-af-interface)#summary-address 0.0.0.0 0.0.0.0
R16(config-router-af-interface)#end
```
#### Проверка
```
R18#ping 10.20.15.17
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.20.15.17, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
R18#ping 10.20.15.16
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.20.15.16, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
R18#sh ip route eigrp
Gateway of last resort is 40.40.19.1 to network 0.0.0.0
      10.0.0.0/8 is variably subnetted, 6 subnets, 3 masks
D        10.20.0.0/16 [90/1024640] via 10.20.17.2, 00:03:38, Ethernet0/1
                      [90/1024640] via 10.20.16.2, 00:03:38, Ethernet0/0
```
```
R16#sh ip route eigrp
Gateway of last resort is 0.0.0.0 to network 0.0.0.0
D*    0.0.0.0/0 is a summary, 00:04:37, Null0
      10.0.0.0/8 is variably subnetted, 15 subnets, 4 masks
D        10.20.0.0/16 is a summary, 00:28:31, Null0
D        10.20.15.18/32 [90/1024640] via 10.20.16.1, 00:04:37, Ethernet0/1
D        10.20.15.32/32 [90/1024640] via 10.20.32.2, 00:04:36, Ethernet0/3
D        10.20.17.0/30 [90/1536000] via 10.20.16.1, 00:04:37, Ethernet0/1

```
### Настройка R32
```
R32(config)#router eigrp SPB-EIGRP
R32(config-router)#address-family ipv4 autonomous-system 2042
R32(config-router-af)#network 10.20.0.0 0.0.255.255
R32(config-router-af)#eigrp stub
R32(config-router-af)#end
```
#### Проверка
```
R32#sh ip route eigrp
Gateway of last resort is 10.20.32.1 to network 0.0.0.0
D*    0.0.0.0/0 [90/1024640] via 10.20.32.1, 00:00:18, Ethernet0/0

```
```
R32 получет только маршрут по умолчанию
```
