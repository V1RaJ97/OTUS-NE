# Лабораторная работа - iBGP
## Схема сети
![alt-text](https://github.com/V1RaJ97/OTUS-NE/blob/7cb30c4bd6739b318e20dd84b65e308da00f04f9/Professional/Labs/iBGP/%D0%A2%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F.png)
## Цели:
1. Настроить iBGP в офисе Москва
2. Настроить iBGP в сети провайдера Триада
3. Организовать полную IP связанность всех сетей

## Описание
- Настройка iBGP в офисом Москва между маршрутизаторами R14 и R15.
- Настройка iBGP в провайдере Триада, с использованием RR.
- Настройка офиса Москва так, чтобы приоритетным провайдером стал Ламас.
- Настройка офиса С.-Петербург так, чтобы трафик до любого офиса распределялся по двум линкам одновременно.
- Все сети в лабораторной работе должны иметь IP связность.
- План работы и изменения зафиксированы в документации.

## Настройка
### Настройка iBGP между R14 и R15
```
R14(config)#router bgp 1001
R14(config-router)#neighbor 10.10.15.15 remote-as 1001
R14(config-router)#neighbor 10.10.15.15 update-source Loopback1

R15(config)#router bgp 1001
R15(config-router)#neighbor 10.10.15.14 remote-as 1001
R15(config-router)#neighbor 10.10.15.14 update-source Loopback1
```
### Настройка iBGP между маршрутизаторами Триада
```
R24(config)#router bgp 520
R24(config-router)#neighbor 40.40.15.23 remote-as 520
R24(config-router)#neighbor 40.40.15.23 route-reflector-client
R24(config-router)#neighbor 40.40.15.23 update-source Loopback1
R24(config-router)#neighbor 40.40.15.25 remote-as 520
R24(config-router)#neighbor 40.40.15.25 route-reflector-client
R24(config-router)#neighbor 40.40.15.25 update-source Loopback1
R24(config-router)#neighbor 40.40.15.26 remote-as 520
R24(config-router)#neighbor 40.40.15.26 route-reflector-cliente
R24(config-router)#neighbor 40.40.15.26 update-source Loopback1
```
```
R26(config)#router bgp 520
R26(config-router)#neighbor 40.40.15.23 remote-as 520
R26(config-router)#neighbor 40.40.15.23 route-reflector-client
R26(config-router)#neighbor 40.40.15.23 update-source Loopback1
R26(config-router)#neighbor 40.40.15.24 remote-as 520
R26(config-router)#neighbor 40.40.15.24 route-reflector-client
R26(config-router)#neighbor 40.40.15.24 update-source Loopback1
R26(config-router)#neighbor 40.40.15.25 remote-as 520
R26(config-router)#neighbor 40.40.15.25 route-reflector-client
R26(config-router)#neighbor 40.40.15.25 update-source Loopback1
R26(config-router)#network 40.40.29.0 mask 255.255.255.252
```
```
R23(config)#router bgp 520
R23(config-router)#neighbor 40.40.15.24 remote-as 520
R23(config-router)#neighbor 40.40.15.24 route-reflector-client
R23(config-router)#neighbor 40.40.15.24 update-source Loopback1
R23(config-router)#neighbor 40.40.15.25 remote-as 520
R23(config-router)#neighbor 40.40.15.25 route-reflector-client
R23(config-router)#neighbor 40.40.15.25 update-source Loopback1
R23(config-router)#neighbor 40.40.15.26 remote-as 520
R23(config-router)#neighbor 40.40.15.26 route-reflector-client
R23(config-router)#neighbor 40.40.15.26 update-source Loopback1
```
```
R25(config)#router bgp 520
R25(config-router)#neighbor 40.40.15.23 remote-as 520
R25(config-router)#neighbor 40.40.15.23 route-reflector-client
R25(config-router)#neighbor 40.40.15.23 update-source Loopback1
R25(config-router)#neighbor 40.40.15.24 remote-as 520
R25(config-router)#neighbor 40.40.15.24 route-reflector-client
R25(config-router)#neighbor 40.40.15.24 update-source Loopback1
R25(config-router)#neighbor 40.40.15.26 remote-as 520
R25(config-router)#neighbor 40.40.15.26 route-reflector-client
R25(config-router)#neighbor 40.40.15.26 route-reflector-client
R25(config-router)#neighbor 40.40.15.26 update-source Loopback1
R25(config-router)#network 40.40.27.0 mask 255.255.255.252
R25(config-router)#network 40.40.28.0 mask 255.255.255.252

```
#### Настройка eBGP между R26 и R18
```
R26(config)#router bgp 520
R26(config-router)#neighbor 40.40.19.2 remote-as 2042
```
```
R18(config)#router bgp 2042
R18(config-router)#neighbor 40.40.19.1 remote-as 520
R18(config-router)#network 40.40.19.0 mask 255.255.255.252
```
#### Проверка маршрутов
```
R14>sh ip route bgp
Gateway of last resort is 30.30.14.1 to network 0.0.0.0

      20.0.0.0/16 is subnetted, 1 subnets
B        20.20.0.0 [200/0] via 20.20.15.1, 01:17:43
      30.0.0.0/8 is variably subnetted, 3 subnets, 3 masks
B        30.30.0.0/16 [20/0] via 30.30.14.1, 01:27:10
      40.0.0.0/8 is variably subnetted, 5 subnets, 2 masks
B        40.40.0.0/16 [20/0] via 30.30.14.1, 00:21:35
B        40.40.19.0/30 [20/0] via 30.30.14.1, 00:21:04
B        40.40.27.0/30 [20/0] via 30.30.14.1, 00:12:54
B        40.40.28.0/30 [20/0] via 30.30.14.1, 00:12:24
B        40.40.29.0/30 [20/0] via 30.30.14.1, 00:21:35
```
```
R24#sh ip bgp nei 40.40.15.25 routes
BGP table version is 14, local router ID is 40.40.15.24
     Network          Next Hop            Metric LocPrf Weight Path
 * i 10.10.15.0/24    40.40.22.2               0    100      0 101 1001 i
 * i 10.10.111.0/24   40.40.22.2               0    100      0 101 1001 i
 * i 10.10.112.0/24   40.40.22.2               0    100      0 101 1001 i
 * i 30.30.0.0/16     40.40.22.2               0    100      0 101 i
 *>i 40.40.27.0/30    40.40.15.25              0    100      0 i
 *>i 40.40.28.0/30    40.40.15.25              0    100      0 i
 * i 40.40.29.0/30    40.40.15.26              0    100      0 i
```
### Настройка приоритетов в Москве и Питере
```
R14(config)#router ospf 1
R14(config-router)#default-information originate metric 255
```
####
```
R13#sh ip route
Gateway of last resort is 10.10.13.1 to network 0.0.0.0

O*E2  0.0.0.0/0 [110/1] via 10.10.13.1, 02:11:01, Ethernet0/2
      10.0.0.0/8 is variably subnetted, 19 subnets, 3 masks
```
```
R12#sh ip route
Gateway of last resort is 10.10.27.1 to network 0.0.0.0

O*E2  0.0.0.0/0 [110/1] via 10.10.27.1, 02:13:42, Ethernet0/3
      10.0.0.0/8 is variably subnetted, 19 subnets, 3 masks
```
```
R18#sh ip route 0.0.0.0
Routing entry for 0.0.0.0/0, supernet
  Known via "static", distance 1, metric 0, candidate default path
  Redistributing via eigrp 2042
  Advertised by eigrp 2042
  Routing Descriptor Blocks:
    40.40.19.1
      Route metric is 0, traffic share count is 1
  * 40.40.18.1
      Route metric is 0, traffic share count is 1

```
