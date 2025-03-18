# Лабораторная работа - OSPF. Фильтрация 
## Схема сети
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/c7e10c42deb75c5c2969e7365c882f5c12e70bf3/Professional/Labs/OSPF/OSPF%20MSK.png)
## Цель
1. Настроить OSPF офисе Москва
2. Разделить сеть на зоны
3. Настроить фильтрацию между зонами
## Описание
1. Маршрутизаторы R14-R15 находятся в зоне 0 - backbone.
2. Маршрутизаторы R12-R13 находятся в зоне 10. Дополнительно к маршрутам должны получать маршрут по умолчанию.
3. Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию.
4. Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101.

| OSPF | Area | Type Area |  Router ID  |
|------|------|-----------|-------------|
| R12  | 10   | Standard  | 10.10.15.12 |
| R13  | 10   | Standard  | 10.10.15.13 |
| R14  | 0    | Backbone  | 10.10.15.14 |
| R14  | 10   | Standard  | 10.10.15.14 |
| R14  | 101  | TotalStub | 10.10.15.14 |
| R15  | 0    | Backbone  | 10.10.15.15 |
| R15  | 10   | Standard  | 10.10.15.15 |
| R15  | 101  | TotalStub | 10.10.15.15 |
| R19  | 101  | Stub      | 10.10.15.19 |
| R20  | 102  | Standard  | 10.10.15.20 |

## Выполнение
```
Прописываем статические маршруты на R14 и R15 до провайдеров Киторн и Ламас:
R14(config)#ip route 0.0.0.0 0.0.0.0 30.30.14.1
R15(config)#ip route 0.0.0.0 0.0.0.0 20.20.15.1
```
### Настраиваем backbone area
```
R14(config)#router ospf 1
R14(config-router)#default-information originate
R14(config-router)#exit
R14(config)#int range e0/0,e0/1,e0/3
R14(config-if-range)#ip ospf 1 area 0
R14(config-if-range)#ip ospf network point-to-point
R14(config-if-range)#exit
R14(config)#int loopback1
R14(config-if)#ip ospf 1 area 0
```
```
R15(config)#router ospf 1
R15(config-router)#default-information originate
R15(config-router)#exit
R15(config)#int range e0/0,e0/1,e0/3
R15(config-if-range)#ip ospf 1 area 0
R15(config-if-range)#ip ospf network point-to-point
R15(config-if-range)#exit
R15(config)#int loopback1
R15(config-if)#ip ospf 1 area 0
```
```
R12(config)#int range e0/2-3
R12(config-if-range)#ip ospf 1 area 0
R12(config-if-range)#ip ospf network point-to-point
```
```
R13(config)#int range e0/2-3
R13(config-if-range)#ip ospf 1 area 0
R13(config-if-range)#ip ospf network point-to-point
```
### Настраиваем standart area 10
```
R12(config)#int e0/0.30
R12(config-subif)#ip ospf 1 area 10
R12(config-subif)#int e0/0.111
R12(config-subif)#ip ospf 1 area 10
R12(config-subif)#int e0/0.112
R12(config-subif)#ip ospf 1 area 10
R12(config-subif)#exit
R12(config)#router ospf 1
R12(config-router)#passive
R12(config-router)#passive-interface e0/0.30
R12(config-router)#passive-interface e0/0.111
R12(config-router)#passive-interface e0/0.112
R12(config-router)#exit
R12(config)#int loopback1
R12(config-if)#ip ospf 1 area 10
R12(config-if)#end
```
```
R13(config)#int e0/0.30
R13(config-subif)#ip ospf 1 area 10
R13(config-subif)#int e0/0.111
R13(config-subif)#ip ospf 1 area 10
R13(config-subif)#int e0/0.112
R13(config-subif)#ip ospf 1 area 10
R13(config-subif)#exit
R13(config)#router ospf 1
R13(config-router)#pass
R13(config-router)#passive-interface e0/0.30
R13(config-router)#passive-interface e0/0.111
R13(config-router)#passive-interface e0/0.112
R13(config-router)#exit
R13(config)#int loopback1
R13(config-if)#ip ospf 1 area 10
R13(config-if)#end
```
#### Проверка
```
R14#sh ip route 10.10.111.4
Routing entry for 10.10.111.0/24
  Known via "ospf 1", distance 110, metric 20, type inter area
  Last update from 10.10.14.2 on Ethernet0/1, 00:17:07 ago
  Routing Descriptor Blocks:
    10.10.14.2, from 10.10.15.13, 00:17:07 ago, via Ethernet0/1
      Route metric is 20, traffic share count is 1
  * 10.10.12.2, from 10.10.15.12, 00:20:35 ago, via Ethernet0/0
      Route metric is 20, traffic share count is 1
```
```
R15#sh ip route 10.10.112.4
Routing entry for 10.10.112.0/24
  Known via "ospf 1", distance 110, metric 20, type inter area
  Last update from 10.10.13.2 on Ethernet0/0, 00:18:35 ago
  Routing Descriptor Blocks:
  * 10.10.27.2, from 10.10.15.12, 00:21:50 ago, via Ethernet0/1
      Route metric is 20, traffic share count is 1
    10.10.13.2, from 10.10.15.13, 00:18:35 ago, via Ethernet0/0
      Route metric is 20, traffic share count is 1
```
### Настраиваем totally stub area 101
```
R19(config)#router ospf 1
R19(config-router)#area 101 stub
R19(config-router)#exit
R19(config)#int loopback1
R19(config-if)#ip ospf 1 area 101
R19(config-if)#exit
R19(config)#int e0/0
R19(config-if)#ip ospf 1 area 101
R19(config-if)#exit
```
```
R14(config)#int e0/3
R14(config-if)#ip ospf 1 area 101
R14(config-if)#exit
R14(config)#router ospf 1
R14(config-router)#area 101 stub no-summary
```
#### Проверка
```
Результат show ip protocols на R14:
Routing Protocol is "ospf 1"
  Outgoing update filter list for all interfaces is not set
  Incoming update filter list for all interfaces is not set
  Router ID 10.10.15.14
  It is an area border and autonomous system boundary router
 Redistributing External Routes from,
  Number of areas in this router is 2. 1 normal 1 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
  Routing on Interfaces Configured Explicitly (Area 0):
    Loopback1
    Ethernet0/1
    Ethernet0/0
  Routing on Interfaces Configured Explicitly (Area 101):
    Ethernet0/3
  Routing Information Sources:
    Gateway         Distance      Last Update
    10.10.15.15          110      00:00:42
    10.10.15.13          110      00:00:52
    10.10.15.12          110      00:00:52
    10.10.15.19          110      00:00:42
  Distance: (default is 110)
```
```
R19#sh ip route
Gateway of last resort is 10.10.19.2 to network 0.0.0.0
O*IA  0.0.0.0/0 [110/11] via 10.10.19.2, 00:04:51, Ethernet0/0
      10.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
C        10.10.15.19/32 is directly connected, Loopback1
C        10.10.19.0/30 is directly connected, Ethernet0/0
L        10.10.19.1/32 is directly connected, Ethernet0/0

```

### Настраиваем standart area 102
```
R15(config)#int e0/3
R15(config-if)#ip ospf 1 area 102
```
```
R20(config)#int e0/0
R20(config-if)#ip ospf 1 area 102
```
```
R15(config)#ip prefix-list area-102 deny 10.10.19.0/30
R15(config)#ip prefix-list area-102 deny 10.10.15.19/32
R15(config)#P:Q
R15(config)#router ospf 1
R15(config-router)#area 102 filter-list prefix area-102 in
R15(config-router)#exit
```
#### Проверка
```
R20#sh ip route 10.10.15.19
% Subnet not in table
R20#sh ip route 10.10.19.1
% Subnet not in table
```
```
R13#sh ip route 10.10.19.1 | inc via
  Known via "ospf 1", distance 110, metric 20, type inter area
  * 10.10.14.1, from 10.10.15.14, 00:24:48 ago, via Ethernet0/3
```
### Конфиги [тут](https://github.com/V1RaJ97/OTUS-NE/tree/d667ea010e611cf3cea10296088745fe64966f3f/Professional/Labs/OSPF/Configs).
