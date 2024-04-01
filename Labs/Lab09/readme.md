# Лабораторная работа - Конфигурация безопасности коммутатора 
## Топология
![alt lext](https://github.com/V1RaJ97/OTUS-NE/blob/6922f1da93b805877a8d3bd7729058cc0962078a/Labs/Lab09/%D0%A2%D0%BE%D0%BF%D0%BE%D0%BB%D0%BE%D0%B3%D0%B8%D1%8F.png)

## Таблица адресации
| Устройство |  Интерфейс/VLAN  |     IP-адрес   | Маска подсети |
|:----------:|:----------------:|:--------------:|:-------------:|
|     R1     |      G0/0/1      |  192.168.10.1  | 255.255.255.0 |
|     R1     |    Loopback 0    |    10.10.1.1   | 255.255.255.0 |
|     S1     |      VLAN 10     | 192.168.10.201 | 255.255.255.0 |
|     S2     |      VLAN 10     | 192.168.10.202 | 255.255.255.0 |
|    PC-A    |        NIC       |      DHCP      | 255.255.255.0 |
|    PC-B    |        NIC       |      DHCP      | 255.255.255.0 |

## Цели
1. Настройка основного сетевого устройства
2. Настройка сетей VLAN
3. Настройки безопасности коммутатора.

## Часть 1. Настройка основного сетевого устройства
### Конфигурация маршрутизатора R1
```
R1#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES unset  administratively down down 
GigabitEthernet0/0/1   192.168.10.1    YES manual up                    up 
Loopback0              10.10.1.1       YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
```
