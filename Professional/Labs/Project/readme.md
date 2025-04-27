# Проектная работа. Создание сетевой инфраструктуры для крупной IT-компании
## Схема сети
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/2b9f1db39846713a930c9d45ff4086cc3f86f3e6/Professional/Labs/Project/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0%20%D1%81%D0%B5%D1%82%D0%B8.png)
## Филиалы
### Москва
```
Московский офис (AS 1001)
Сеть IPv4 10.10.0.0/16
10.10.0.1 - 10.10.255.254
```
|    VLAN    |     Имя     |      Сеть     |
|:----------:|:-----------:|:-------------:|
|     30     | Management  | 10.10.30.0/24 |
|     111    | MSK-SBER1/2 | 10.10.111.0/24|
|     112    | MSK-ALFA1/2 | 10.10.112.0/24|
|     113    | MSK-VTB1/2  | 10.10.113.0/24|

|  Устройство  |  Интерфейс |   IP-адрес    |      Маска      |    Шлюз    |
|--------------|------------|---------------|-----------------|------------|
| MSK-CORE1    |    Lo1     | 10.10.15.21   | 255.255.255.255 |            |
| MSK-CORE1    |    e0/0    | 10.10.11.1    | 255.255.255.252 |            |
| MSK-CORE1    |    e0/1    | 11.11.10.2    | 255.255.255.252 |            |
| MSK-CORE1    |    e0/2    | 10.10.12.1    | 255.255.255.252 |            |
| MSK-CORE2    |    Lo1     | 10.10.15.22   | 255.255.255.255 |            |
| MSK-CORE2    |    e0/0    | 10.10.22.1    | 255.255.255.252 |            |
| MSK-CORE2    |    e0/1    | 11.12.10.2    | 255.255.255.252 |            |
| MSK-CORE2    |    e0/2    | 10.10.21.1    | 255.255.255.252 |            |
| MSK-R1       |    Lo1     | 10.10.15.11   | 255.255.255.255 |            |
| MSK-R1       |    e0/0    | 10.10.11.2    | 255.255.255.252 |            |
| MSK-R1       |    e0/2    | 10.10.21.2    | 255.255.255.252 |            |
| MSK-R1       |  e0/1.30   | 10.10.30.1    | 255.255.255.0   |            |
| MSK-R1       |  e0/1.111  | 10.10.111.1   | 255.255.255.0   |            |
| MSK-R1       |  e0/1.112  | 10.10.112.1   | 255.255.255.0   |            |
| MSK-R1       |  e0/1.113  | 10.10.113.1   | 255.255.255.0   |            |
| MSK-R2       |    Lo1     | 10.10.15.12   | 255.255.255.255 |            |
| MSK-R2       |    e0/0    | 10.10.22.2    | 255.255.255.252 |            |
| MSK-R2       |    e0/2    | 10.10.12.2    | 255.255.255.252 |            |
| MSK-R2       |  e0/1.30   | 10.10.30.2    | 255.255.255.0   |            |
| MSK-R2       |  e0/1.111  | 10.10.111.2   | 255.255.255.0   |            |
| MSK-R2       |  e0/1.112  | 10.10.112.2   | 255.255.255.0   |            |
| MSK-R2       |  e0/1.113  | 10.10.113.2   | 255.255.255.0   |            |
| MSK-SW1      |   VLAN 30  | 10.10.30.11   | 255.255.255.0   |            |
| MSK-SW2      |   VLAN 30  | 10.10.30.12   | 255.255.255.0   |            |
| MSK-SW3      |   VLAN 30  | 10.10.30.13   | 255.255.255.0   |            |
| MSK-SW4      |   VLAN 30  | 10.10.30.14   | 255.255.255.0   |            |
| MSK-SBER1    |   Eth0     |               | 255.255.255.0   |            |
| MSK-SBER2    |   Eth0     |               | 255.255.255.0   |            |
| MSK-ALFA1    |   Eth0     |               | 255.255.255.0   |            |
| MSK-ALFA2    |   Eth0     |               | 255.255.255.0   |            |
| MSK-VTB1     |   Eth0     |               | 255.255.255.0   |            |
| MSK-VTB2     |   Eth0     |               | 255.255.255.0   |            |

#### Провайдеры
##### MSK-ISP1 (AS 101)
```
Сеть IPv4 11.11.0.0/16
11.11.0.1 - 11.11.255.254
```
|  Устройство  |  Интерфейс |   IP-адрес    |      Маска      |    Шлюз    |
|--------------|------------|---------------|-----------------|------------|
| MSK-ISP1     |    Lo1     | 11.11.15.11   | 255.255.255.255 |            |
| MSK-ISP1     |    e0/0    | 11.11.10.1    | 255.255.255.252 |            |
| MSK-ISP1     |    e0/1    | 11.40.113.2   | 255.255.255.252 |            |

##### MSK-ISP2 (AS 102)
```
Сеть IPv4 11.12.0.0/16
11.12.0.1 - 11.12.255.254
```
|  Устройство  |  Интерфейс |   IP-адрес    |      Маска      |    Шлюз    |
|--------------|------------|---------------|-----------------|------------|
| MSK-ISP2     |    Lo1     | 11.12.15.12   | 255.255.255.255 |            |
| MSK-ISP2     |    e0/0    | 11.12.10.1    | 255.255.255.252 |            |
| MSK-ISP2     |    e0/1    | 11.40.114.2   | 255.255.255.252 |            |


### Санкт-Петербург
```
Питерский офис (AS 2002)
Сеть IPv4 10.20.0.0/16
10.20.0.1 - 10.20.255.254
```
|    VLAN    |     Имя     |      Сеть     |
|:----------:|:-----------:|:-------------:|
|     30     | Management  | 10.20.30.0/24 |
|     111    | SPB-SBER1/2 | 10.20.111.0/24|
|     112    | SPB-ALFA1/2 | 10.20.112.0/24|
|     113    | SPB-VTB1/2  | 10.20.113.0/24|

|  Устройство  |  Интерфейс |   IP-адрес    |      Маска      |    Шлюз    |
|--------------|------------|---------------|-----------------|------------|
| SPB-CORE1    |    Lo1     | 10.20.15.21   | 255.255.255.255 |            |
| SPB-CORE1    |    e0/0    | 11.21.10.2    | 255.255.255.252 |            |
| SPB-CORE1    |    e0/1    | 10.20.11.1    | 255.255.255.252 |            |
| SPB-CORE1    |    e0/2    | 10.20.21.1    | 255.255.255.252 |            |
| SPB-CORE2    |    Lo1     | 10.20.15.22   | 255.255.255.255 |            |
| SPB-CORE2    |    e0/0    | 11.22.10.2    | 255.255.255.252 |            |
| SPB-CORE2    |    e0/1    | 10.20.12.1    | 255.255.255.252 |            |
| SPB-CORE2    |    e0/2    | 10.20.22.1    | 255.255.255.252 |            |
| SPB-R1       |    Lo1     | 10.20.15.11   | 255.255.255.255 |            |
| SPB-R1       |    e0/1    | 10.20.11.2    | 255.255.255.252 |            |
| SPB-R1       |    e0/2    | 10.20.22.2    | 255.255.255.252 |            |
| SPB-R1       |  e0/0.30   | 10.20.30.1    | 255.255.255.0   |            |
| SPB-R1       |  e0/0.111  | 10.20.111.1   | 255.255.255.0   |            |
| SPB-R1       |  e0/0.112  | 10.20.112.1   | 255.255.255.0   |            |
| SPB-R1       |  e0/0.113  | 10.20.113.1   | 255.255.255.0   |            |
| SPB-R2       |    Lo1     | 10.20.15.12   | 255.255.255.255 |            |
| SPB-R2       |    e0/1    | 10.20.12.2    | 255.255.255.252 |            |
| SPB-R2       |    e0/2    | 10.20.21.2    | 255.255.255.252 |            |
| SPB-R2       |  e0/0.30   | 10.20.30.2    | 255.255.255.0   |            |
| SPB-R2       |  e0/0.111  | 10.20.111.2   | 255.255.255.0   |            |
| SPB-R2       |  e0/0.112  | 10.20.112.2   | 255.255.255.0   |            |
| SPB-R2       |  e0/0.113  | 10.20.113.2   | 255.255.255.0   |            |
| SPB-SW1      |   VLAN 30  | 10.20.30.11   | 255.255.255.0   |            |
| SPB-SW2      |   VLAN 30  | 10.20.30.12   | 255.255.255.0   |            |
| SPB-SW3      |   VLAN 30  | 10.20.30.13   | 255.255.255.0   |            |
| SPB-SW4      |   VLAN 30  | 10.20.30.14   | 255.255.255.0   |            |
| SPB-SBER1    |   Eth0     |               | 255.255.255.0   |            |
| SPB-SBER2    |   Eth0     |               | 255.255.255.0   |            |
| SPB-ALFA1    |   Eth0     |               | 255.255.255.0   |            |
| SPB-ALFA2    |   Eth0     |               | 255.255.255.0   |            |
| SPB-VTB1     |   Eth0     |               | 255.255.255.0   |            |
| SPB-VTB2     |   Eth0     |               | 255.255.255.0   |            |

#### Провайдеры
##### SPB-ISP1 (AS 201)
```
Сеть IPv4 11.21.0.0/16
11.21.0.1 - 11.21.255.254
```
|  Устройство  |  Интерфейс |   IP-адрес    |      Маска      |    Шлюз    |
|--------------|------------|---------------|-----------------|------------|
| SPB-ISP1     |    Lo1     | 11.21.15.11   | 255.255.255.255 |            |
| SPB-ISP1     |    e0/0    | 11.21.10.1    | 255.255.255.252 |            |
| SPB-ISP1     |    e0/1    | 11.40.115.2   | 255.255.255.252 |            |

##### SPB-ISP2 (AS 202)
```
Сеть IPv4 11.22.0.0/16
11.22.0.1 - 11.22.255.254
```
|  Устройство  |  Интерфейс |   IP-адрес    |      Маска      |    Шлюз    |
|--------------|------------|---------------|-----------------|------------|
| SPB-ISP2     |    Lo1     | 11.22.15.12   | 255.255.255.255 |            |
| SPB-ISP2     |    e0/0    | 11.22.10.1    | 255.255.255.252 |            |
| SPB-ISP2     |    e0/1    | 11.40.112.2   | 255.255.255.252 |            |

### Омск
```
Омский офис (AS 3003)
Сеть IPv4 10.30.0.0/16
10.30.0.1 - 10.30.255.254
```
|    VLAN    |     Имя     |      Сеть     |
|:----------:|:-----------:|:-------------:|
|     30     | Management  | 10.30.30.0/24 |
|     111    | OMSK-SBER1/2| 10.30.111.0/24|
|     112    | OMSK-ALFA1/2| 10.30.112.0/24|

|  Устройство  |  Интерфейс |   IP-адрес    |      Маска      |    Шлюз    |
|--------------|------------|---------------|-----------------|------------|
| OMSK-CORE1   |    Lo1     | 10.30.15.21   | 255.255.255.255 |            |
| OMSK-CORE1   |    e0/0    | 11.31.10.2    | 255.255.255.252 |            |
| OMSK-CORE1   |    e0/1    | 10.30.11.1    | 255.255.255.252 |            |
| OMSK-CORE1   |    e0/2    | 10.30.21.1    | 255.255.255.252 |            |
| OMSK-CORE2   |    Lo1     | 10.30.15.22   | 255.255.255.255 |            |
| OMSK-CORE2   |    e0/0    | 11.32.10.2    | 255.255.255.252 |            |
| OMSK-CORE2   |    e0/1    | 10.30.12.1    | 255.255.255.252 |            |
| OMSK-CORE2   |    e0/2    | 10.30.22.1    | 255.255.255.252 |            |
| OMSK-R1      |    Lo1     | 10.30.15.11   | 255.255.255.255 |            |
| OMSK-R1      |    e0/0    | 10.30.11.2    | 255.255.255.252 |            |
| OMSK-R1      |    e0/2    | 10.30.22.2    | 255.255.255.252 |            |
| OMSK-R1      |  e0/1.30   | 10.30.30.1    | 255.255.255.0   |            |
| OMSK-R1      |  e0/1.111  | 10.30.111.1   | 255.255.255.0   |            |
| OMSK-R1      |  e0/1.112  | 10.30.112.1   | 255.255.255.0   |            |
| OMSK-R2      |    Lo1     | 10.30.15.12   | 255.255.255.255 |            |
| OMSK-R2      |    e0/0    | 10.30.12.2    | 255.255.255.252 |            |
| OMSK-R2      |    e0/2    | 10.30.21.2    | 255.255.255.252 |            |
| OMSK-R2      |  e0/1.30   | 10.30.30.2    | 255.255.255.0   |            |
| OMSK-R2      |  e0/1.111  | 10.30.111.2   | 255.255.255.0   |            |
| OMSK-R2      |  e0/1.112  | 10.30.112.2   | 255.255.255.0   |            |
| OMSK-SW1     |   VLAN 30  | 10.30.30.11   | 255.255.255.0   |            |
| OMSK-SW2     |   VLAN 30  | 10.30.30.12   | 255.255.255.0   |            |
| OMSK-SBER1   |   Eth0     |               | 255.255.255.255 |            |
| OMSK-SBER2   |   Eth0     |               | 255.255.255.255 |            |
| OMSK-ALFA1   |   Eth0     |               | 255.255.255.255 |            |
| OMSK-ALFA2   |   Eth0     |               | 255.255.255.255 |            |

#### Провайдеры
##### OMSK-ISP1 (AS 301)
```
Сеть IPv4 11.31.0.0/16
11.31.0.1 - 11.31.255.254
```
|  Устройство  |  Интерфейс |   IP-адрес    |      Маска      |    Шлюз    |
|--------------|------------|---------------|-----------------|------------|
| OMSK-ISP1    |    Lo1     | 11.31.15.11   | 255.255.255.255 |            |
| OMSK-ISP1    |    e0/0    | 11.31.10.1    | 255.255.255.252 |            |
| OMSK-ISP1    |    e0/1    | 11.40.111.2   | 255.255.255.252 |            |

##### OMSK-ISP2 (AS 302)
```
Сеть IPv4 11.32.0.0/16
11.32.0.1 - 11.32.255.254
```
|  Устройство  |  Интерфейс |   IP-адрес    |      Маска      |    Шлюз    |
|--------------|------------|---------------|-----------------|------------|
| OMSK-ISP2    |    Lo1     | 11.32.15.12   | 255.255.255.255 |            |
| OMSK-ISP2    |    e0/0    | 11.32.10.1    | 255.255.255.252 |            |
| OMSK-ISP2    |    e0/1    | 11.40.116.2   | 255.255.255.252 |            |

### Global Network (AS 401)
```
Сеть IPv4 11.40.0.0/16
11.40.0.1 - 11.40.255.254
```
|  Устройство  |  Интерфейс |   IP-адрес    |      Маска      |    Шлюз    |
|--------------|------------|---------------|-----------------|------------|
| GN-R1        |    Lo1     | 11.40.15.11   | 255.255.255.255 |            |
| GN-R1        |    e0/0    | 11.40.12.1    | 255.255.255.252 |            |
| GN-R1        |    e0/1    | 11.40.13.1    | 255.255.255.252 |            |
| GN-R1        |    e0/2    | 11.40.111.1   | 255.255.255.252 |            |
| GN-R1        |    e0/3    | 11.40.117.1   | 255.255.255.252 |            |
| GN-R1        |    e1/0    | 11.40.11.1    | 255.255.255.252 |            |
| GN-R2        |    Lo1     | 11.40.15.12   | 255.255.255.255 |            |
| GN-R2        |    e0/0    | 11.40.12.2    | 255.255.255.252 |            |
| GN-R2        |    e0/1    | 11.40.14.1    | 255.255.255.252 |            |
| GN-R2        |    e0/2    | 11.40.112.1   | 255.255.255.252 |            |
| GN-R2        |    e0/3    | 11.40.118.1   | 255.255.255.252 |            |
| GN-R2        |    e1/0    | 11.40.16.1    | 255.255.255.252 |            |
| GN-R3        |    Lo1     | 11.40.15.13   | 255.255.255.255 |            |
| GN-R3        |    e0/0    | 11.40.13.2    | 255.255.255.252 |            |
| GN-R3        |    e0/1    | 11.40.24.1    | 255.255.255.252 |            |
| GN-R3        |    e0/2    | 11.40.113.1   | 255.255.255.252 |            |
| GN-R3        |    e0/3    | 11.40.116.1   | 255.255.255.252 |            |
| GN-R3        |    e1/0    | 11.40.16.2    | 255.255.255.252 |            |
| GN-R4        |    Lo1     | 11.40.15.14   | 255.255.255.255 |            |
| GN-R4        |    e0/0    | 11.40.14.2    | 255.255.255.252 |            |
| GN-R4        |    e0/1    | 11.40.24.2    | 255.255.255.252 |            |
| GN-R4        |    e0/2    | 11.40.114.1   | 255.255.255.252 |            |
| GN-R4        |    e0/3    | 11.40.115.1   | 255.255.255.252 |            |
| GN-R4        |    e1/0    | 11.40.11.2    | 255.255.255.252 |            |

### Data Center (AS 5001)
```
Сеть IPv4 10.50.0.0/16
10.50.0.1 - 10.50.255.254
```
|    VLAN    |     Имя     |      Сеть     |
|:----------:|:-----------:|:-------------:|
|     30     | Management  | 10.50.30.0/24 |
|     100    |   Servers   | 10.50.100.0/24|

|  Устройство  |  Интерфейс |   IP-адрес    |      Маска      |     Шлюз    |
|--------------|------------|---------------|-----------------|-------------|
| DC-R1        |    Lo1     | 10.50.15.11   | 255.255.255.255 |             |
| DC-R1        |    e0/1    | 11.40.117.2   | 255.255.255.252 |             |
| DC-R1        |    e0/2    | 11.40.118.2   | 255.255.255.252 |             |
| DC-R1        |   e0/0.30  | 10.50.30.1    | 255.255.255.0   |             |
| DC-R1        |   e0/0.100 | 10.50.100.1   | 255.255.255.0   |             |
| DC-SW1       |   VLAN 30  | 10.50.30.11   | 255.255.255.0   |             |
| DC-SV-SBER   |    Eth0    | 10.50.100.111 | 255.255.255.0   | 10.50.100.1 |
| DC-SV-ALFA   |    Eth0    | 10.50.100.112 | 255.255.255.0   | 10.50.100.1 |
| DC-SV-VTB    |    Eth0    | 10.50.100.113 | 255.255.255.0   | 10.50.100.1 |

## Настройка BGP
### iBGP
```
GN-R1(config)#router bgp 401
GN-R1(config-router)#bgp log-neighbor-changes
GN-R1(config-router)#neighbor 11.40.12.2 remote-as 401
GN-R1(config-router)#neighbor 11.40.13.2 remote-as 401
GN-R1(config-router)#neighbor 11.40.12.2 next-hop-self
GN-R1(config-router)#neighbor 11.40.13.2 next-hop-self
GN-R1(config-router)#neighbor 11.40.11.2 remote-as 401
GN-R1(config-router)#neighbor 11.40.11.2 next-hop-self
GN-R1(config-router)#network 0.0.0.0 mask 0.0.0.0

```
```
GN-R2(config)#router bgp 401
GN-R2(config-router)#bgp log-neighbor-changes
GN-R2(config-router)#neighbor 11.40.12.1 remote-as 401
GN-R2(config-router)#neighbor 11.40.14.2 remote-as 401
GN-R2(config-router)#neighbor 11.40.12.1 next-hop-self
GN-R2(config-router)#neighbor 11.40.14.2 next-hop-self
GN-R2(config-router)#neighbor 11.40.16.2 remote-as 401
GN-R2(config-router)#neighbor 11.40.16.2 next-hop-self
GN-R2(config-router)#network 0.0.0.0 mask 0.0.0.0


```
```
GN-R3(config)#router bgp 401
GN-R3(config-router)#bgp log-neighbor-changes
GN-R3(config-router)#neighbor 11.40.13.1 remote-as 401
GN-R3(config-router)#neighbor 11.40.13.1 next-hop-self
GN-R3(config-router)#neighbor 11.40.24.2 remote-as 401
GN-R3(config-router)#neighbor 11.40.24.2 next-hop-self
GN-R3(config-router)#neighbor 11.40.16.1 remote-as 401
GN-R3(config-router)#neighbor 11.40.16.1 next-hop-self
GN-R3(config-router)#network 0.0.0.0 mask 0.0.0.0


```
```
GN-R4(config)#router bgp 401
GN-R4(config-router)#bgp log-neighbor-changes
GN-R4(config-router)#neighbor 11.40.14.1 remote-as 401
GN-R4(config-router)#neighbor 11.40.14.1 next-hop-self
GN-R4(config-router)#neighbor 11.40.24.1 remote-as 401
GN-R4(config-router)#neighbor 11.40.24.1 next-hop-self
GN-R4(config-router)#neighbor 11.40.11.1 next-hop-se
GN-R4(config-router)#neighbor 11.40.11.1 next-hop-self
GN-R4(config-router)#network 0.0.0.0 mask 0.0.0.0


```
```
GN-R1(config)#router bgp 401
GN-R1(config-router)#neighbor 11.40.14.2 remote-as 401
GN-R1(config-router)#neighbor 11.40.14.2 ebgp-multihop 2


GN-R4(config)#router bgp 401
GN-R4(config-router)#neighbor 11.40.12.1 remote-as 401
GN-R4(config-router)#neighbor 11.40.12.1 ebgp-multihop 2



```
### eBGP
#### Между GN и ISP
```
GN-R1(config)#router bgp 401
GN-R1(config-router)#neighbor 11.40.111.2 remote-as 301
GN-R1(config-router)#neighbor 11.40.117.2 remote-as 5001
```
```
GN-R2(config)#router bgp 401
GN-R2(config-router)#neighbor 11.40.118.2 remote-as 5001
GN-R2(config-router)#neighbor 11.40.112.2 remote-as 202
```
```
GN-R3(config)#router bgp 401
GN-R3(config-router)#neighbor 11.40.116.2 remote-as 302
GN-R3(config-router)#neighbor 11.40.113.2 remote-as 101
```
```
GN-R4(config)#router bgp 401
GN-R4(config-router)#neighbor 11.40.114.2 remote-as 102
GN-R4(config-router)#neighbor 11.40.115.2 remote-as 201
```
```
OMSK-ISP1(config)#router bgp 301
OMSK-ISP1(config-router)#neighbor 11.40.111.1 remote-as 401
OMSK-ISP1(config-router)#network 0.0.0.0 mask 0.0.0.0

OMSK-ISP2(config)#router bgp 302
OMSK-ISP2(config-router)#neighbor 11.40.116.1 remote-as 401
OMSK-ISP2(config-router)#network 0.0.0.0 mask 0.0.0.0


MSK-ISP1(config)#router bgp 101
MSK-ISP1(config-router)#neighbor 11.40.113.1 remote-as 401
MSK-ISP1(config-router)#network 0.0.0.0 mask 0.0.0.0


MSK-ISP2(config)#router bgp 102
MSK-ISP2(config-router)#neighbor 11.40.114.1 remote-as 401
MSK-ISP2(config-router)#network 0.0.0.0 mask 0.0.0.0


SPB-ISP1(config)#router bgp 201
SPB-ISP1(config-router)#neighbor 11.40.115.1 remote-as 401
SPB-ISP1(config-router)#network 0.0.0.0 mask 0.0.0.0


SPB-ISP2(config)#router bgp 202
SPB-ISP2(config-router)#neighbor 11.40.112.1 remote-as 401
SPB-ISP2(config-router)#network 0.0.0.0 mask 0.0.0.0


DC-R1(config)#router bgp 5001
DC-R1(config-router)#neighbor 11.40.117.1 remote-as 401
DC-R1(config-router)#neighbor 11.40.118.1 remote-as 401
```
#### Между ISP и Офисами
```
MSK-ISP1(config)#router bgp 101
MSK-ISP1(config-router)#neighbor 11.11.10.2 remote-as 1001

MSK-CORE1(config)#router bgp 1001
MSK-CORE1(config-router)#neighbor 11.11.10.1 remote-as 101
```
```
MSK-ISP2(config)#router bgp 102
MSK-ISP2(config-router)#neighbor 11.12.10.2 remote-as 1001

MSK-CORE2(config)#router bgp 1001
MSK-CORE2(config-router)#neighbor 11.12.10.1 remote-as 102
```
```
SPB-ISP1(config)#router bgp 201
SPB-ISP1(config-router)#neighbor 11.21.10.2 remote-as 2002

SPB-CORE1(config)#router bgp 2002
SPB-CORE1(config-router)#neighbor 11.21.10.1 remote-as 201
```
```
SPB-ISP2(config)#router bgp 202
SPB-ISP2(config-router)#neighbor 11.22.10.2 remote-as 2002

SPB-CORE2(config)#router bgp 2002
SPB-CORE2(config-router)#neighbor 11.22.10.1 remote-as 202
```
```
OMSK-ISP1(config)#router bgp 301
OMSK-ISP1(config-router)#neighbor 11.31.10.2 remote-as 3003

OMSK-CORE1(config)#router bgp 3003
OMSK-CORE1(config-router)#neighbor 11.31.10.1 remote-as 301
```
```
OMSK-ISP2(config)#router bgp 302
OMSK-ISP2(config-router)#neighbor 11.32.10.2 remote-as 3003

OMSK-CORE2(config)#router bgp 3003
OMSK-CORE2(config-router)#neighbor 11.32.10.1 remote-as 302

```
## NAT(PAT)
```
MSK-CORE1(config)#ip access-list standard NAT-INSIDE
MSK-CORE1(config-std-nacl)#permit 10.10.0.0 0.0.255.255
MSK-CORE1(config)#int e0/1
MSK-CORE1(config-if)#ip nat outside
MSK-CORE1(config-if)#exit
MSK-CORE1(config)#int range e0/0,e0/2
MSK-CORE1(config-if-range)#ip nat inside
MSK-CORE1(config)#ip nat inside source list NAT-INSIDE int e0/1 overload
```
```
MSK-CORE2(config)#ip access-list standard NAT-INSIDE
MSK-CORE2(config-std-nacl)#permit 10.10.0.0 0.0.255.255
MSK-CORE2(config)#int e0/1
MSK-CORE2(config-if)#ip nat outside
MSK-CORE2(config-if)#exit
MSK-CORE2(config)#int range e0/0,e0/2
MSK-CORE2(config-if-range)#ip nat inside
MSK-CORE2(config)#ip nat inside source list NAT-INSIDE int e0/1 overload
```
```
SPB-CORE1(config)#ip access-list standard NAT-INSIDE
SPB-CORE1(config-std-nacl)#permit 10.20.0.0 0.0.255.255
SPB-CORE1(config-std-nacl)#exit
SPB-CORE1(config)#int e0/0
SPB-CORE1(config-if)#ip nat outside
SPB-CORE1(config-if)#exit
SPB-CORE1(config)#int range e0/1-2
SPB-CORE1(config-if-range)#ip nat inside
SPB-CORE1(config-if-range)#exit
SPB-CORE1(config)#ip nat inside source list NAT-INSIDE int e0/0 overload
```
```
SPB-CORE2(config)#ip access-list standard NAT-INSIDE
SPB-CORE2(config-std-nacl)#permit 10.20.0.0 0.0.255.255
SPB-CORE2(config-std-nacl)#exit
SPB-CORE2(config)#int e0/0
SPB-CORE2(config-if)#ip nat outside
SPB-CORE2(config-if)#exit
SPB-CORE2(config)#int range e0/1-2
SPB-CORE2(config-if-range)#ip nat inside
SPB-CORE2(config-if-range)#exit
SPB-CORE2(config)#ip nat inside source list NAT-INSIDE int e0/0 overload
```
```
OMSK-CORE1(config)#ip access-list standard NAT-INSIDE
OMSK-CORE1(config-std-nacl)#permit 10.30.0.0 0.0.255.255
OMSK-CORE1(config-std-nacl)#exit
OMSK-CORE1(config)#int e0/0
OMSK-CORE1(config-if)#ip nat outside
OMSK-CORE1(config-if)#exit
OMSK-CORE1(config)#int range e0/1-2
OMSK-CORE1(config-if-range)#ip nat inside
OMSK-CORE1(config-if-range)#exit
OMSK-CORE1(config)#ip nat inside source list NAT-INSIDE int e0/0 overload
```
```
OMSK-CORE2(config)#ip access-list standard NAT-INSIDE
OMSK-CORE2(config-std-nacl)#permit 10.30.0.0 0.0.255.255
OMSK-CORE2(config-std-nacl)#exit
OMSK-CORE2(config)#int e0/0
OMSK-CORE2(config-if)#ip nat outside
OMSK-CORE2(config-if)#exit
OMSK-CORE2(config)#int range e0/1-2
OMSK-CORE2(config-if-range)#ip nat inside
OMSK-CORE2(config-if-range)#exit
OMSK-CORE2(config)#ip nat inside source list NAT-INSIDE int e0/0 overload
```
## DMVPN OVER IPSEC Phase 3
### MSK-CORE1/2 (Hubs)
```
MSK-CORE1(config)#crypto isakmp policy 10
MSK-CORE1(config-isakmp)#encr aes 256
MSK-CORE1(config-isakmp)#hash sha256
MSK-CORE1(config-isakmp)#authentication pre-share
MSK-CORE1(config-isakmp)#group 14
MSK-CORE1(config-isakmp)#crypto isakmp key 0 Cisco123 address 0.0.0.0 0.0.0.0
MSK-CORE1(config)#crypto ipsec transform-set TS-SET esp-aes 256 esp-sha256-hmac
MSK-CORE1(cfg-crypto-trans)#mode transport
MSK-CORE1(cfg-crypto-trans)#exit
MSK-CORE1(config)#crypto ipsec profile DMVPN-PROFILE
MSK-CORE1(ipsec-profile)#set transform-set TS-SET

MSK-CORE1(config)#interface Tunnel0
MSK-CORE1(config-if)#ip address 10.1.1.1 255.255.255.0
MSK-CORE1(config-if)#tunnel source e0/1
MSK-CORE1(config-if)#tunnel mode gre multipoint
MSK-CORE1(config-if)#tunnel key 100
MSK-CORE1(config-if)#tunnel protection ipsec profile DMVPN-PROFILE
MSK-CORE1(config-if)#ip nhrp authentication dmvpn
MSK-CORE1(config-if)#ip nhrp map multicast dynamic
MSK-CORE1(config-if)#ip nhrp network-id 100
MSK-CORE1(config-if)#ip nhrp redirect
```
```
MSK-CORE2(config)#crypto isakmp policy 10
MSK-CORE2(config-isakmp)#encr aes 256
MSK-CORE2(config-isakmp)#hash sha256
MSK-CORE2(config-isakmp)#authentication pre-share
MSK-CORE2(config-isakmp)#group 14
MSK-CORE2(config-isakmp)#crypto isakmp key 0 Cisco123 address 0.0.0.0 0.0.0.0
MSK-CORE2(config)#crypto ipsec transform-set TS-SET esp-aes 256 esp-sha256-hmac
MSK-CORE2(cfg-crypto-trans)#mode transport
MSK-CORE2(cfg-crypto-trans)#exit
MSK-CORE2(config)#crypto ipsec profile DMVPN-PROFILE
MSK-CORE2(ipsec-profile)#set transform-set TS-SET

MSK-CORE2(config)#interface Tunnel0
MSK-CORE2(config-if)#ip address 10.1.1.2 255.255.255.0
MSK-CORE2(config-if)#tunnel source e0/1
MSK-CORE2(config-if)#tunnel mode gre multipoint
MSK-CORE2(config-if)#tunnel key 100
MSK-CORE2(config-if)#tunnel protection ipsec profile DMVPN-PROFILE
MSK-CORE2(config-if)#ip nhrp authentication dmvpn
MSK-CORE2(config-if)#ip nhrp map multicast dynamic
MSK-CORE2(config-if)#ip nhrp network-id 100
MSK-CORE2(config-if)#ip nhrp redirect
```
### Spokes
```
SPB-CORE1(config)#crypto isakmp policy 10
SPB-CORE1(config-isakmp)#encr aes 256
SPB-CORE1(config-isakmp)#hash sha256
SPB-CORE1(config-isakmp)#authentication pre-share
SPB-CORE1(config-isakmp)#group 14
SPB-CORE1(config-isakmp)#crypto isakmp key 0 Cisco123 address 0.0.0.0 0.0.0.0
SPB-CORE1(config)#crypto ipsec transform-set TS-SET esp-aes 256 esp-sha256-hmac
SPB-CORE1(cfg-crypto-trans)#mode transport
SPB-CORE1(cfg-crypto-trans)#exit
SPB-CORE1(config)#crypto ipsec profile DMVPN-PROFILE
SPB-CORE1(ipsec-profile)#set transform-set TS-SET
SPB-CORE1(ipsec-profile)#exit

SPB-CORE1(config)#interface Tunnel0
SPB-CORE1(config-if)#ip address 10.1.1.21 255.255.255.0
SPB-CORE1(config-if)#tunnel source e0/0
SPB-CORE1(config-if)#tunnel mode gre multipoint
SPB-CORE1(config-if)#tunnel key 100
SPB-CORE1(config-if)#tunnel protection ipsec profile DMVPN-PROFILE
SPB-CORE1(config-if)#ip nhrp authentication dmvpn
SPB-CORE1(config-if)#ip nhrp map 10.1.1.1 11.11.10.2
SPB-CORE1(config-if)#ip nhrp nhs 10.1.1.1
SPB-CORE1(config-if)#ip nhrp network-id 100
SPB-CORE1(config-if)#ip nhrp map multicast 11.11.10.2
SPB-CORE1(config-if)#ip nhrp shortcut
SPB-CORE1(config-if)#ip nhrp map 10.1.1.2 11.12.10.2
SPB-CORE1(config-if)#ip nhrp nhs 10.1.1.2
SPB-CORE1(config-if)#ip nhrp map multicast 11.12.10.2
```
```
SPB-CORE2(config)#crypto isakmp policy 10
SPB-CORE2(config-isakmp)#encr aes 256
SPB-CORE2(config-isakmp)#hash sha256
SPB-CORE2(config-isakmp)#authentication pre-share
SPB-CORE2(config-isakmp)#group 14
SPB-CORE2(config-isakmp)#crypto isakmp key 0 Cisco123 address 0.0.0.0 0.0.0.0
SPB-CORE2(config)#crypto ipsec transform-set TS-SET esp-aes 256 esp-sha256-hmac
SPB-CORE2(cfg-crypto-trans)#mode transport
SPB-CORE2(cfg-crypto-trans)#exit
SPB-CORE2(config)#crypto ipsec profile DMVPN-PROFILE
SPB-CORE2(ipsec-profile)#set transform-set TS-SET
SPB-CORE2(ipsec-profile)#exit

SPB-CORE2(config)#interface Tunnel0
SPB-CORE2(config-if)#ip address 10.1.1.22 255.255.255.0
SPB-CORE2(config-if)#tunnel source e0/0
SPB-CORE2(config-if)#tunnel mode gre multipoint
SPB-CORE2(config-if)#tunnel key 100
SPB-CORE2(config-if)#tunnel protection ipsec profile DMVPN-PROFILE
SPB-CORE2(config-if)#ip nhrp authentication dmvpn
SPB-CORE2(config-if)#ip nhrp map 10.1.1.1 11.11.10.2
SPB-CORE2(config-if)#ip nhrp nhs 10.1.1.1
SPB-CORE2(config-if)#ip nhrp map multicast 11.11.10.2
SPB-CORE2(config-if)#ip nhrp shortcut
SPB-CORE2(config-if)#ip nhrp network-id 100
SPB-CORE2(config-if)#ip nhrp map 10.1.1.2 11.12.10.2
SPB-CORE2(config-if)#ip nhrp nhs 10.1.1.2
SPB-CORE2(config-if)#ip nhrp map multicast 11.12.10.2
```
```
OMSK-CORE1(config)#crypto isakmp policy 10
OMSK-CORE1(config-isakmp)#encr aes 256
OMSK-CORE1(config-isakmp)#hash sha256
OMSK-CORE1(config-isakmp)#authentication pre-share
OMSK-CORE1(config-isakmp)#group 14
OMSK-CORE1(config-isakmp)#crypto isakmp key 0 Cisco123 address 0.0.0.0 0.0.0.0
OMSK-CORE1(config)#crypto ipsec transform-set TS-SET esp-aes 256 esp-sha256-hmac
OMSK-CORE1(cfg-crypto-trans)#mode transport
OMSK-CORE1(cfg-crypto-trans)#exit
OMSK-CORE1(config)#crypto ipsec profile DMVPN-PROFILE
OMSK-CORE1(ipsec-profile)#set transform-set TS-SET
OMSK-CORE1(ipsec-profile)#exit

OMSK-CORE1(config)#interface Tunnel0
OMSK-CORE1(config-if)#ip address 10.1.1.31 255.255.255.0
OMSK-CORE1(config-if)#tunnel source e0/0
OMSK-CORE1(config-if)#tunnel mode gre multipoint
OMSK-CORE1(config-if)#tunnel key 100
OMSK-CORE1(config-if)#tunnel protection ipsec profile DMVPN-PROFILE
OMSK-CORE1(config-if)#ip nhrp authentication dmvpn
OMSK-CORE1(config-if)#ip nhrp map 10.1.1.1 11.11.10.2
OMSK-CORE1(config-if)#ip nhrp nhs 10.1.1.1
OMSK-CORE1(config-if)#ip nhrp map multicast 11.11.10.2
OMSK-CORE1(config-if)#ip nhrp shortcut
OMSK-CORE1(config-if)#ip nhrp network-id 100
OMSK-CORE1(config-if)#ip nhrp map 10.1.1.2 11.12.10.2
OMSK-CORE1(config-if)#ip nhrp nhs 10.1.1.2
OMSK-CORE1(config-if)#ip nhrp map multicast 11.12.10.2
```
```
OMSK-CORE2(config)#crypto isakmp policy 10
OMSK-CORE2(config-isakmp)#encr aes 256
OMSK-CORE2(config-isakmp)#hash sha256
OMSK-CORE2(config-isakmp)#authentication pre-share
OMSK-CORE2(config-isakmp)#group 14
OMSK-CORE2(config-isakmp)#crypto isakmp key 0 Cisco123 address 0.0.0.0 0.0.0.0
OMSK-CORE2(config)#crypto ipsec transform-set TS-SET esp-aes 256 esp-sha256-hmac
OMSK-CORE2(cfg-crypto-trans)#mode transport
OMSK-CORE2(cfg-crypto-trans)#exit
OMSK-CORE2(config)#crypto ipsec profile DMVPN-PROFILE
OMSK-CORE2(ipsec-profile)#set transform-set TS-SET
OMSK-CORE2(ipsec-profile)#exit

OMSK-CORE2(config)#interface Tunnel0
OMSK-CORE2(config-if)#ip address 10.1.1.32 255.255.255.0
OMSK-CORE2(config-if)#tunnel source e0/0
OMSK-CORE2(config-if)#tunnel mode gre multipoint
OMSK-CORE2(config-if)#tunnel key 100
OMSK-CORE2(config-if)#tunnel protection ipsec profile DMVPN-PROFILE
OMSK-CORE2(config-if)#ip nhrp authentication dmvpn
OMSK-CORE2(config-if)#ip nhrp map 10.1.1.1 11.11.10.2
OMSK-CORE2(config-if)#ip nhrp nhs 10.1.1.1
OMSK-CORE2(config-if)#ip nhrp map multicast 11.11.10.2
OMSK-CORE2(config-if)#ip nhrp shortcut
OMSK-CORE2(config-if)#ip nhrp network-id 100
OMSK-CORE2(config-if)#ip nhrp map 10.1.1.2 11.12.10.2
OMSK-CORE2(config-if)#ip nhrp nhs 10.1.1.2
OMSK-CORE2(config-if)#ip nhrp map multicast 11.12.10.2
```
```
DC-R1(config)#crypto isakmp policy 10
DC-R1(config-isakmp)#encr aes 256
DC-R1(config-isakmp)#hash sha256
DC-R1(config-isakmp)#authentication pre-share
DC-R1(config-isakmp)#group 14
DC-R1(config-isakmp)#crypto isakmp key 0 Cisco123 address 0.0.0.0 0.0.0.0
DC-R1(config)#crypto ipsec transform-set TS-SET esp-aes 256 esp-sha256-hmac
DC-R1(cfg-crypto-trans)#mode transport
DC-R1(cfg-crypto-trans)#exit
DC-R1(config)#crypto ipsec profile DMVPN-PROFILE
DC-R1(ipsec-profile)#set transform-set TS-SET
DC-R1(ipsec-profile)#exit
DC-R1(config)#interface Tunnel0
DC-R1(config-if)#ip address 10.1.1.51 255.255.255.0
DC-R1(config-if)#tunnel source e0/1
DC-R1(config-if)#tunnel mode gre multipoint
DC-R1(config-if)#tunnel key 100
DC-R1(config-if)#tunnel protection ipsec profile DMVPN-PROFILE
DC-R1(config-if)#ip nhrp authentication dmvpn
DC-R1(config-if)#ip nhrp map 10.1.1.1 11.11.10.2
DC-R1(config-if)#ip nhrp nhs 10.1.1.1
DC-R1(config-if)#ip nhrp map multicast 11.11.10.2
DC-R1(config-if)#ip nhrp shortcut
DC-R1(config-if)#ip nhrp network-id 100
DC-R1(config-if)#ip nhrp map 10.1.1.2 11.12.10.2
DC-R1(config-if)#ip nhrp nhs 10.1.1.2
DC-R1(config-if)#ip nhrp map multicast 11.12.10.2
DC-R1(config-if)#exit
DC-R1(config)#interface Tunnel1
DC-R1(config-if)#ip address 10.1.2.51 255.255.255.0
DC-R1(config-if)#tunnel source e0/2
DC-R1(config-if)#tunnel mode gre multipoint
DC-R1(config-if)#tunnel key 100
DC-R1(config-if)#tunnel protection ipsec profile DMVPN-PROFILE
DC-R1(config-if)#ip nhrp authentication dmvpn
DC-R1(config-if)#ip nhrp map 10.1.1.1 11.11.10.2
DC-R1(config-if)#ip nhrp nhs 10.1.1.1
DC-R1(config-if)#ip nhrp map multicast 11.11.10.2
DC-R1(config-if)#ip nhrp shortcut
DC-R1(config-if)#ip nhrp network-id 100
DC-R1(config-if)#ip nhrp map 10.1.1.2 11.12.10.2
DC-R1(config-if)#ip nhrp nhs 10.1.1.2
DC-R1(config-if)#ip nhrp map multicast 11.12.10.2

```
