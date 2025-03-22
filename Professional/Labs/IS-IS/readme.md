# Лабораторная работа - IS-IS
## Цель
Настроить IS-IS офисе Триада

## Описание
1. Настроить IS-IS в ISP Триада.
2. R23 и R25 находятся в зоне 2222.
3. R24 находится в зоне 24.
4. R26 находится в зоне 26.

## Топология
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/00461f1e5707bca46131d3c5755e697065ff9b3d/Professional/Labs/IS-IS/triada.png)

| Устройство | Интерфейс |    IP-адрес    |
| :--------: | :-------: | :------------: |
| R23        | Loopback1 | 40.40.15.23/32 |
| R24        | Loopback1 | 40.40.15.24/32 |
| R25        | Loopback1 | 40.40.15.25/32 |
| R26        | Loopback1 | 40.40.15.26/32 |


### Преобразовение IP-адресов в NET:

```
40.40.15.23 -> 040.040.015.023 -> 0400.4001.5023
40.40.15.24 -> 040.040.015.024 -> 0400.4001.5024
40.40.15.25 -> 040.040.015.025 -> 0400.4001.5025
40.40.15.26 -> 040.040.015.026 -> 0400.4001.5026
```


| Устройство |   Network Entity Title    |
| :--------: | :-----------------------: |
| R23        | 49.2222.0400.4001.5023.00 |
| R24        | 49.24.0400.4001.5024.00   |
| R25        | 49.2222.0400.4001.5025.00 |
| R26        | 49.26.0400.4001.5026.00   |

| Устройство | Роль |
| :--------- | :--- |
| R23        | L1L2 |
| R24        | L2   |
| R25        | L1L2 |
| R26        | L2   |

## Настройка R23 и R25
```
R23(config)#router isis
R23(config-router)#net 49.2222.0400.4001.5023.00
R23(config-router)#is-type level-1-2
R23(config-router)#exit
R23(config)#int range e0/1-2,loopback1
R23(config-if-range)#ip router isis
R23(config-if-range)#end
```
```
R25(config)#router isis
R25(config-router)#net 49.2222.0400.4001.5025.00
R25(config-router)#is-type level-1-2
R25(config-router)#exit
R25(config)#int range e0/0,e0/2,loopback1
R25(config-if-range)#ip router isis
R25(config-if-range)#end
```
### Проверка
```
R25#sh ip route isis
Gateway of last resort is not set
      40.0.0.0/8 is variably subnetted, 11 subnets, 2 masks
i L1     40.40.15.23/32 [115/20] via 40.40.25.1, 00:03:15, Ethernet0/0
i L1     40.40.24.0/30 [115/20] via 40.40.25.1, 00:03:15, Ethernet0/0
```
```
R23#sh ip route isis
Gateway of last resort is not set
      40.0.0.0/8 is variably subnetted, 9 subnets, 2 masks
i L1     40.40.15.25/32 [115/20] via 40.40.25.2, 00:04:11, Ethernet0/1
i L1     40.40.26.0/30 [115/20] via 40.40.25.2, 00:04:11, Ethernet0/1
```
## Настройка R24 и R26
```
R24(config)#router isis
R24(config-router)#net 49.24.0400.4001.5024.00
R24(config-router)#is-type level-2-only
R24(config-router)#exit
R24(config)#int range e0/1-2,loopback1
R24(config-if-range)#ip router isis
R24(config-if-range)#end
```
```
R26(config)#router isis
R26(config-router)#net 49.26.0400.4001.5026.00
R26(config-router)#exit
R26(config)#int range e0/0,e0/2,loopback1
R26(config-if-range)#ip router isis
R26(config-if-range)#end
```
### Проверка
```
R25#sh isi nei
System Id      Type Interface   IP Address      State Holdtime Circuit Id
R23            L1   Et0/0       40.40.25.1      UP    25       R25.01           
R23            L2   Et0/0       40.40.25.1      UP    24       R25.01           
R26            L2   Et0/2       40.40.26.2      UP    7        R26.02  
```
```
R24#sh ip route isis
Gateway of last resort is not set
      40.0.0.0/8 is variably subnetted, 14 subnets, 2 masks
i L2     40.40.15.23/32 [115/20] via 40.40.24.1, 00:13:14, Ethernet0/2
i L2     40.40.15.25/32 [115/30] via 40.40.24.1, 00:02:05, Ethernet0/2
                        [115/30] via 40.40.20.2, 00:02:05, Ethernet0/1
i L2     40.40.15.26/32 [115/20] via 40.40.20.2, 00:02:05, Ethernet0/1
i L2     40.40.25.0/30 [115/20] via 40.40.24.1, 00:13:14, Ethernet0/2
i L2     40.40.26.0/30 [115/20] via 40.40.20.2, 00:02:05, Ethernet0/1
```
## Проверка работы ISIS
До разрыва линка между R24 и R26
```
R24#sh isis topology
IS-IS TID 0 paths to level-2 routers
System Id            Metric     Next-Hop             Interface   SNPA
R23                  10         R23                  Et0/2       aabb.cc01.7020
R24                  --
R25                  20         R23                  Et0/2       aabb.cc01.7020
                                R26                  Et0/1       aabb.cc01.a000
R26                  10         R26                  Et0/1       aabb.cc01.a000
```
После разрыва линка между R24 и R26
```
R24#sh isis topology
IS-IS TID 0 paths to level-2 routers
System Id            Metric     Next-Hop             Interface   SNPA
R23                  10         R23                  Et0/2       aabb.cc01.7020
R24                  --
R25                  20         R23                  Et0/2       aabb.cc01.7020
R26                  30         R23                  Et0/2       aabb.cc01.7020
```
