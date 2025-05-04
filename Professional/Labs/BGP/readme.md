# Лабораторная работа - BGP. Основы
## Схема сети
![alt-text](https://github.com/V1RaJ97/OTUS-NE/blob/3421c1e71c2ab5da5b0bca1391606b86f4bcdb38/Professional/Labs/BGP/bgp.png)
## Цель
Настроить BGP между автономными системами и организовать доступность между офисами Москва и С.-Петербург
## Описание
- Настроить eBGP между офисом Москва и двумя провайдерами - Киторн и Ламас.
- Настроить eBGP между провайдерами Киторн и Ламас.
- Настроить eBGP между Ламас и Триада.
- Настроить eBGP между офисом С.-Петербург и провайдером Триада.
- Организовать IP доступность между пограничным роутерами офисами Москва и С.-Петербург.
- План работы и изменения зафиксировать в документации.

### Настройка eBGP между Московским офисом и провайдерами Киторн и Ламас
```
R14(config)#router bgp 1001
R14(config-router)#neighbor 30.30.14.1 remote-as 101
R14(config-router)#network 10.10.15.0 mask 255.255.255.0
R14(config-router)#network 10.10.111.0 mask 255.255.255.0
R14(config-router)#network 10.10.112.0 mask 255.255.255.0
R14(config-router)#exit
R14(config)#ip route 10.10.15.0 255.255.255.0 null 0

R22(config)#router bgp 101
R22(config-router)#neighbor 30.30.14.2 remote-as 1001

```
```
R15(config)#router bgp 1001
R15(config-router)#neighbor 20.20.15.1 remote-as 301
R15(config-router)#network 10.10.15.0 mask 255.255.255.0
R15(config-router)#network 10.10.111.0 mask 255.255.255.0
R15(config-router)#network 10.10.112.0 mask 255.255.255.0
R15(config-router)#exit
R15(config)#ip route 10.10.15.0 255.255.255.0 null 0

R21(config)#router bgp 301
R21(config-router)#neighbor 20.20.15.2 remote-as 1001
```
### Настройка eBGP между провайдерами Киторн и Ламас
```
R22(config)#router bgp 101
R22(config-router)#neighbor 30.30.22.2 remote-as 301

R21(config)#router bgp 301
R21(config-router)#neighbor 30.30.22.1 remote-as 101
```
### Настроика eBGP между Ламас и Триада
```
R21(config)#router bgp 301
R21(config-router)#neighbor 40.40.21.1 remote-as 520

R24(config)#router bgp 520
R24(config-router)#neighbor 40.40.21.2 remote-as 301
```
### Настройка eBGP между Триада С.-Петербург
```
R24(config)#router bgp 520
R24(config-router)#neighbor 40.40.18.2 remote-as 2042

R18(config)#router bgp 2042
R18(config-router)#neighbor 40.40.18.1 remote-as 520
R18(config-router)#network 10.20.15.0 mask 255.255.255.0



```
