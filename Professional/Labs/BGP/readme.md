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
R22(config-router)#network 30.30.0.0 mask 255.255.0.0
R22(config-router)#exit
R22(config)#ip route 30.30.0.0 255.255.0.0 null 0

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
R21(config-router)#network 20.20.0.0 mask 255.255.0.0
R21(config-router)#exit
R21(config)#ip route 20.20.0.0 255.255.0.0 null 0
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
R24(config-router)#network 40.40.0.0 mask 255.255.0.0
R24(config-router)#exit
R24(config)#ip route 40.40.0.0 255.255.0.0 Null0


R18(config)#router bgp 2042
R18(config-router)#neighbor 40.40.18.1 remote-as 520
R18(config-router)#network 10.20.15.0 mask 255.255.255.0
```
### Проверка
#### Маршруты на R24
```
R24#sh ip bgp
BGP table version is 11, local router ID is 40.40.15.24
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>  10.10.15.0/24    40.40.21.2                             0 301 1001 i
 *>  10.10.111.0/24   40.40.21.2                             0 301 1001 i
 *>  10.10.112.0/24   40.40.21.2                             0 301 1001 i
 *>  20.20.0.0/16     40.40.21.2               0             0 301 i
 *>  30.30.0.0/16     40.40.21.2                             0 301 101 i
 *>  40.40.0.0/16     0.0.0.0                  0         32768 i
```
#### Результаты ping с Московских роутеров до R18
```
R14#ping 40.40.18.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 40.40.18.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```
```
R15#ping 40.40.18.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 40.40.18.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```
#### Результаты ping с R18 до Московских роутеров
```
R18#ping 30.30.14.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 30.30.14.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
R18#ping 20.20.15.2
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 20.20.15.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```
### Конфиги [тут](https://github.com/V1RaJ97/OTUS-NE/tree/57ccf554ef7ef71638f5a13aecc118277f802e6c/Professional/Labs/BGP/Configs).
