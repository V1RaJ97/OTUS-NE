# Проектная работа. Создание сетевой инфраструктуры для IT-компании
## Топология
## Таблица адресации
| Устройство |     Интерфейс    |    IP-адрес    | Маска подсети | Шлюз по умолчанию |
|:----------:|:----------------:|:--------------:|:-------------:|:-----------------:|
|     R1     |      G0/0/1      |   _________    | _____________ |                   |
|     R1     |     G0/0/1.10    |   10.10.0.1    | 255.255.255.0 |                   |
|     R1     |     G0/0/1.20    |   10.20.0.1    | 255.255.255.0 |                   | 
|     R1     |     G0/0/1.30    |   10.30.0.1    | 255.255.255.0 |                   |
|     R1     |     G0/0/1.40    |   10.40.0.1    | 255.255.255.0 |                   |
|     R1     |     G0/0/1.50    |   10.50.0.1    | 255.255.255.0 |                   |
|     R1     |     G0/0/1.60    |   10.60.0.1    | 255.255.255.0 |                   |
|     R1     |     G0/0/1.70    |   10.70.0.1    | 255.255.255.0 |                   |



## Таблица VLAN
| VLAN |      Имя     |         Назначенный интерфейс         |
|:----:|:------------:|:-------------------------------------:|
|  10  |  Management  |                                       |
|  20  |Infrastructure|                                       |
|  30  |  Accounting  |                                       |
|  40  |      HR      |                                       |
|  50  |     SBER     |                                       |
|  60  |      VTB     |                                       |
|  70  |      GPB     |                                       |
|  999 | Parking_Lot  |                                       |
| 1000 |   Private    |                                       |


### Создание сабинтерфейсов на R1
```
R1(config)#int g0/0/1.10
R1(config-subif)#encapsulation dot1Q 10
R1(config-subif)#ip address 10.10.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.20
R1(config-subif)#encapsulation dot1Q 20
R1(config-subif)#ip address 10.20.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.30
R1(config-subif)#encapsulation dot1Q 30
R1(config-subif)#ip address 10.30.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.40
R1(config-subif)#encapsulation dot1Q 40
R1(config-subif)#ip address 10.40.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.50
R1(config-subif)#encapsulation dot1Q 50
R1(config-subif)#ip address 10.50.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.60
R1(config-subif)#encapsulation dot1Q 60
R1(config-subif)#ip address 10.60.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.70
R1(config-subif)#encapsulation dot1Q 70
R1(config-subif)#ip address 10.70.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.1000
R1(config-subif)#encapsulation dot1Q 1000 native
R1(config-subif)#exit
```
```
R1#show ip interface brief 
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES unset  administratively down down 
GigabitEthernet0/0/1   unassigned      YES unset  up                    up 
GigabitEthernet0/0/1.1010.10.0.1       YES manual up                    up 
GigabitEthernet0/0/1.2010.20.0.1       YES manual up                    up 
GigabitEthernet0/0/1.3010.30.0.1       YES manual up                    up 
GigabitEthernet0/0/1.4010.40.0.1       YES manual up                    up 
GigabitEthernet0/0/1.5010.50.0.1       YES manual up                    up 
GigabitEthernet0/0/1.6010.60.0.1       YES manual up                    up 
GigabitEthernet0/0/1.7010.70.0.1       YES manual up                    up 
GigabitEthernet0/0/1.1000unassigned      YES unset  up                    up 
GigabitEthernet0/0/2   unassigned      YES unset  administratively down down 
Vlan1                  unassigned      YES unset  administratively down down
```
