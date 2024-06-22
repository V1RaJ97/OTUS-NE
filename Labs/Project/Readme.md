# Проектная работа. Создание сетевой инфраструктуры для IT-компании
## Топология
## Таблица адресации
| Устройство |     Интерфейс    |    IP-адрес    | Маска подсети | Шлюз по умолчанию |
|:----------:|:----------------:|:--------------:|:-------------:|:-----------------:|
|     R1     |      G0/0/1      |   _________    | _____________ |                   |
|     R1     |      G0/1.10     |   10.10.0.1    | 255.255.255.0 |                   |
|     R1     |      G0/1.20     |   10.20.0.1    | 255.255.255.0 |                   | 
|     R1     |      G0/1.30     |   10.30.0.1    | 255.255.255.0 |                   |
|     R1     |      G0/1.40     |   10.40.0.1    | 255.255.255.0 |                   |
|     R1     |      G0/1.50     |   10.50.0.1    | 255.255.255.0 |                   |
|     R1     |      G0/1.60     |   10.60.0.1    | 255.255.255.0 |                   |
|     R1     |      G0/1.70     |   10.70.0.1    | 255.255.255.0 |                   |
|     R1     |      G0/1.100    |   10.100.0.1   | 255.255.255.0 |                   |
|     R1     |      G0/1.111    |   10.111.0.1   | 255.255.255.0 |                   |
|     R1     |      G0/1.1000   |   _________    | _____________ |                   |
|     S1     |      VLAN 10     |   10.10.0.11   | 255.255.255.0 |     10.10.0.1     |
|     S2     |      VLAN 10     |   10.10.0.12   | 255.255.255.0 |     10.10.0.1     |
|     S3     |      VLAN 10     |   10.10.0.13   | 255.255.255.0 |     10.10.0.1     |
|     S4     |      VLAN 10     |   10.10.0.14   | 255.255.255.0 |     10.10.0.1     |
|     S5     |      VLAN 10     |   10.10.0.15   | 255.255.255.0 |     10.10.0.1     |



## Таблица VLAN
| VLAN |        Имя        |               Назначение              |                    Используемые порты                 |
|:----:|:-----------------:|:-------------------------------------:|:-----------------------------------------------------:|
|  10  | Network_Management|           Сетевые устройства          |                                                       |
|  20  |   Infrastructure  |                Сервера                |                                                       |
|  30  |     Accounting    |              Бухгалтерия              |                       S2: Fa0/5-6                     |
|  40  |         HR        |                  HR                   |                       S2: Fa0/7-8                     |
|  50  |        SBER       |              Проект Сбер              |                       S4: Fa0/3-4                     |
|  60  |        VTB        |              Проект ВТБ               |                       S4: Fa0/5-6                     |
|  70  |        GPB        |              Проект ГПБ               |                       S4: Fa0/7-8                     |
|  100 |      IT_dept      |               IT отдел                |                       S2: Fa0/3-4                     |
|  111 |        ISP        |       Интернет cервис провайдер       |                                                       |
|  999 |    Parking_Lot    |                                       |S2/S4: Fa0/1-2, Fa0/9-10, Fa0/13-14, Fa0/16-24, Gi0/1-2|
|  999 |    Parking_Lot    |                                       |            S1/S3: Fa0/1-10, Fa0/16-24, Gi0/2          |
| 1000 |      Private      |                                       |                                                       |


### Создание сабинтерфейсов на R1
```
R1(config)#int g0/1.10
R1(config-subif)#encapsulation dot1Q 10
R1(config-subif)#ip address 10.10.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/1.20
R1(config-subif)#encapsulation dot1Q 20
R1(config-subif)#ip address 10.20.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/1.30
R1(config-subif)#encapsulation dot1Q 30
R1(config-subif)#ip address 10.30.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/1.40
R1(config-subif)#encapsulation dot1Q 40
R1(config-subif)#ip address 10.40.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/1.50
R1(config-subif)#encapsulation dot1Q 50
R1(config-subif)#ip address 10.50.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/1.60
R1(config-subif)#encapsulation dot1Q 60
R1(config-subif)#ip address 10.60.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/1.70
R1(config-subif)#encapsulation dot1Q 70
R1(config-subif)#ip address 10.70.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/1.100
R1(config-subif)#encapsulation dot1Q 100
R1(config-subif)#ip address 10.100.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/1.1000
R1(config-subif)#encapsulation dot1Q 1000 native
R1(config-subif)#exit
```
```
R1#show ip interface br
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0     unassigned      YES unset  up                    down 
GigabitEthernet0/1     unassigned      YES unset  up                    down 
GigabitEthernet0/1.10  10.10.0.1       YES manual up                    down 
GigabitEthernet0/1.20  10.20.0.1       YES manual up                    down 
GigabitEthernet0/1.30  10.30.0.1       YES manual up                    down 
GigabitEthernet0/1.40  10.40.0.1       YES manual up                    down 
GigabitEthernet0/1.50  10.50.0.1       YES manual up                    down 
GigabitEthernet0/1.60  10.60.0.1       YES manual up                    down 
GigabitEthernet0/1.70  10.70.0.1       YES manual up                    down 
GigabitEthernet0/1.100 10.100.0.1      YES manual up                    down 
GigabitEthernet0/1.1000unassigned      YES unset  up                    down 
GigabitEthernet0/2     unassigned      YES unset  up                    down 
Vlan1                  unassigned      YES unset  administratively down down
```

### Создание VLAN на коммутаторах S1-4
```
S1(config)#vlan 10
S1(config-vlan)#name Network_Management
S1(config-vlan)#vlan 20
S1(config-vlan)#name Infrastructunre
S1(config-vlan)#vlan 30
S1(config-vlan)#name Accounting
S1(config-vlan)#vlan 40 
S1(config-vlan)#name HR
S1(config-vlan)#vlan 50
S1(config-vlan)#name SBER
S1(config-vlan)#vlan 60
S1(config-vlan)#name VTB
S1(config-vlan)#vlan 70
S1(config-vlan)#name GPB
S1(config-vlan)#vlan 100
S1(config-vlan)#name IT_dept
S1(config-vlan)#vlan 999
S1(config-vlan)#name Parking_Lot
S1(config-vlan)#vlan 1000
S1(config-vlan)#name Private
S1(config-vlan)#exit
```
### Отключение неиспользуемых портов на коммутаторах
#### S1,S3
```
S1(config)#interface range fa0/1-10
S1(config-if-range)#shutdown
S1(config-if-range)#switchport mode access 
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#exit
S1(config)#interface range fa0/16-24
S1(config-if-range)#shutdown
S1(config-if-range)#switchport mode access 
S1(config-if-range)#switchport access vlan 999
S1(config-if-range)#exit
S1(config)#int g0/2
S1(config-if)#shutdown
S1(config-if)#switchport mode access 
S1(config-if)#switchport access vlan 999
S1(config-if)#exit

```
#### S2,S4
```
S2(config)#int range fa0/1-2
S2(config-if-range)#shutdown
S2(config-if-range)#switchport mode access 
S2(config-if-range)#switchport access vlan 999
S2(config-if-range)#exit
S2(config)#int range fa0/9-10
S2(config-if-range)#shutdown 
S2(config-if-range)#switchport mode access 
S2(config-if-range)#switchport access vlan 999
S2(config-if-range)#exit
S2(config)#int range fa0/13-14
S2(config-if-range)#shutdown 
S2(config-if-range)#switchport mode access 
S2(config-if-range)#switchport access vlan 999
S2(config-if-range)#exit
S2(config)#int range fa0/16-24
S2(config-if-range)#shutdown
S2(config-if-range)#switchport mode access
S2(config-if-range)#switchport access vlan 999
S2(config-if-range)#exit
S2(config)#int range gi0/1-2
S2(config-if-range)#shutdown
S2(config-if-range)#switchport mode access 
S2(config-if-range)#switchport access vlan 999
S2(config-if-range)#exit
```
### Настойка Etherchannel
#### S1
```
S1(config)#int vlan 10
S1(config-if)#ip address 10.10.0.11 255.255.255.0
S1(config-if)#exit
S1(config)#ip default-gateway 10.10.0.1
S1(config)#int range fa0/13-14
S1(config-if-range)#channel-group 1 mode active 
S1(config-if-range)#int port-channel 1
S1(config-if)#switchport mode trunk
S1(config-if)#switchport trunk all vlan 10,20,30,40,50,60,70,100,1000
S1(config-if)#switchport trunk native vlan 1000
S1(config-if)#exit
S1(config)#int range fa0/11-12
S1(config-if-range)#channel-group 2 mode active 
S1(config-if-range)#int port-channel 2
S1(config-if)#shutdown
S1(config-if)#switchport mode trunk
S1(config-if)#switchport trunk all vlan 10,20,30,40,50,60,70,100,1000
S1(config-if)#switchport trunk native vlan 1000
S1(config-if)#no shutdown 
S1(config)#int range fa0/1-10
S1(config-if-range)#switchport mode access 
S1(config-if-range)#exit
S1(config)#int range fa0/16-24
S1(config-if-range)#switchport mode access 
S1(config-if-range)#exit
S1(config)#int fa0/15
S1(config-if)#switchport mode trunk
S1(config-if)#switchport trunk allowed vlan 10,20,30,40,50,60,70,100,1000
S1(config-if)#switchport trunk native vlan 1000
S1(config-if)#exit
S1(config)#int g0/1
S1(config-if)#switchport mode trunk
S1(config-if)#switchport trunk allowed vlan 10,20,30,40,50,60,70,100,1000
S1(config-if)#switchport trunk native vlan 1000
S1(config-if)#exit
```
#### S3
```
S3(config)#int vlan 10
S3(config-if)#ip address 10.10.0.13 255.255.255.0
S3(config-if)#exit
S3(config)#ip default-gateway 10.10.0.1
S3(config)#int range fa0/13-14
S3(config-if-range)#shutdown
S3(config-if-range)#channel-group 1 mode active
S3(config-if-range)#exit
S3(config)#int port-channel 1
S3(config-if)#switchport mode trunk 
S3(config-if)#switchport trunk allowed vlan 10,20,30,40,50,60,70,100,1000
S3(config-if)#switchport trunk native vlan 1000
S3(config-if)#exit
S3(config)#int range fa0/13-14
S3(config-if-range)#no shutdown
S3(config-if-range)#exit
S3(config)#int range fa0/11-12
S3(config-if-range)#channel-group 2 mode active
S3(config-if-range)#int port-channel 2
S3(config-if)#shutdown 
S3(config-if)#switchport mode trunk
S3(config-if)#switchport trunk all vlan 10,20,30,40,50,60,70,100,1000
S3(config-if)#switchport trunk native vlan 1000
S3(config-if)#no shutdown 
S3(config-if)#exit
S3(config)#int fa0/15
S3(config-if)#switchport mode trunk
S3(config-if)#switchport trunk allowed vlan 10,20,30,40,50,60,70,100,1000
S3(config-if)#switchport trunk native vlan 1000
S3(config-if)#exit
S3(config)#int g0/1
S3(config-if)#switchport mode trunk
S3(config-if)#switchport trunk allowed vlan 10,20,30,40,50,60,70,100,1000
S3(config-if)#switchport trunk native vlan 1000
S3(config-if)#exit
```
#### S2
```
S2(config)#int vlan 10
S2(config-if)#ip address 10.10.0.12 255.255.255.0
S2(config-if)#exit
S2(config)#ip default-gateway 10.10.0.1
S2(config)#int range fa0/11-12
S2(config-if-range)#channel-group 1 mode active
S2(config-if-range)#int port-channel 1
S2(config-if)#shutdown 
S2(config-if)#switchport mode trunk
S2(config-if)#switchport trunk all vlan 10,20,30,40,50,60,70,100,1000
S2(config-if)#switchport trunk native vlan 1000
S2(config-if)#no shutdown 
S2(config-if)#exit
S2(config)#int fa0/15
S2(config-if)#switchport mode trunk
S2(config-if)#switchport trunk allowed vlan 10,20,30,40,50,60,70,100,1000
S2(config-if)#switchport trunk native vlan 1000
S2(config-if)#exit
```
#### S4
```
S4(config)#int vlan 10
S4(config-if)#ip address 10.10.0.14 255.255.255.0
S4(config-if)#exit
S4(config)#ip default-gateway 10.10.0.1
S4(config)#int range fa0/11-12
S4(config-if-range)#channel-group 1 mode active
S4(config-if-range)#int port-channel 1
S4(config-if)#shutdown 
S4(config-if)#switchport mode trunk
S4(config-if)#switchport trunk all vlan 10,20,30,40,50,60,70,100,1000
S4(config-if)#switchport trunk native vlan 1000
S4(config-if)#no shutdown 
S4(config-if)#exit
S4(config)#int fa0/15
S4(config-if)#switchport mode trunk
S4(config-if)#switchport trunk allowed vlan 10,20,30,40,50,60,70,100,1000
S4(config-if)#switchport trunk native vlan 1000
S4(config-if)#exit
```
### Настойка SPT
```
S1(config)#spanning-tree vlan 10 root primary
S1(config)#spanning-tree mode rapid-pvst 
S3(config)#spanning-tree vlan 10 root secondary
S3(config)#spanning-tree mode rapid-pvst
S2(config)#spanning-tree mode rapid-pvst
S4(config)#spanning-tree mode rapid-pvst 
```
#### Настройка граничных портов
```
S2(config)#int range fa0/3-8
S2(config-if-range)#no shutdown
S2(config-if-range)#switchport mode access
S2(config-if-range)#spanning-tree portfast
S2(config-if-range)#no shutdown 
```
```
S4(config)#int range fa0/3-8
S4(config-if-range)#no shutdown
S4(config-if-range)#switchport mode access
S4(config-if-range)#spanning-tree portfast
S4(config-if-range)#no shutdown 
```
