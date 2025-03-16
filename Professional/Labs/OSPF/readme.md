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
```
R19(config)#int e0/0
R19(config-if)#ip ospf 1 area 0
R19(config-if)#ip ospf network point-to-point
```
```
R20(config)#int e0/0
R20(config-if)#ip ospf 1 area 0
R20(config-if)#ip ospf network point-to-point
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
