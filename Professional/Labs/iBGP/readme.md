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
