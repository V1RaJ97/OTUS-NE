# Проектная работа. Создание сетевой инфраструктуры для IT-компании
## Топология
## Таблица адресации
| Устройство |     Интерфейс    |    IP-адрес    | Маска подсети | Шлюз по умолчанию |
|:----------:|:----------------:|:--------------:|:-------------:|:-----------------:|
|     R1     |       G0/0       |   172.16.1.1   | 255.255.255.0 |                   |
|     R1     |       G0/1       |   _________    | _____________ |                   |
|     R1     |      G0/1.10     |   10.10.0.1    | 255.255.255.0 |                   |
|     R1     |      G0/1.20     |   10.20.0.1    | 255.255.255.0 |                   | 
|     R1     |      G0/1.30     |   10.30.0.1    | 255.255.255.0 |                   |
|     R1     |      G0/1.40     |   10.40.0.1    | 255.255.255.0 |                   |
|     R1     |      G0/1.50     |   10.50.0.1    | 255.255.255.0 |                   |
|     R1     |      G0/1.60     |   10.60.0.1    | 255.255.255.0 |                   |
|     R1     |      G0/1.70     |   10.70.0.1    | 255.255.255.0 |                   |
|     R1     |      G0/1.100    |   10.100.0.1   | 255.255.255.0 |                   |
|     R1     |      G0/1.1000   |   _________    | _____________ |                   |
|     R1     |      G0/2.11     |   10.11.0.1    | 255.255.255.0 |                   |
|     R1     |      G0/2.21     |   10.21.0.1    | 255.255.255.0 |                   | 
|     R2     |       G0/2       |   172.16.2.1   | _____________ |                   |
|     R2     |       G0/1       |   _________    | _____________ |                   |
|     R2     |      G0/1.10     |   10.10.0.2    | 255.255.255.0 |                   |
|     R2     |      G0/1.20     |   10.20.0.2    | 255.255.255.0 |                   | 
|     R2     |      G0/1.30     |   10.30.0.2    | 255.255.255.0 |                   |
|     R2     |      G0/1.40     |   10.40.0.2    | 255.255.255.0 |                   |
|     R2     |      G0/1.50     |   10.50.0.2    | 255.255.255.0 |                   |
|     R2     |      G0/1.60     |   10.60.0.2    | 255.255.255.0 |                   |
|     R2     |      G0/1.70     |   10.70.0.2    | 255.255.255.0 |                   |
|     R2     |      G0/1.100    |   10.100.0.2   | 255.255.255.0 |                   |
|     R2     |      G0/1.1000   |   _________    | _____________ |                   |
|     R2     |      G0/0.11     |   10.11.0.2    | 255.255.255.0 |                   |
|     R2     |      G0/0.21     |   10.21.0.2    | 255.255.255.0 |                   | 
|     S1     |      VLAN 10     |   10.10.0.11   | 255.255.255.0 |     10.10.0.3     |
|     S2     |      VLAN 10     |   10.10.0.12   | 255.255.255.0 |     10.10.0.3     |
|     S3     |      VLAN 10     |   10.10.0.13   | 255.255.255.0 |     10.10.0.3     |
|     S4     |      VLAN 10     |   10.10.0.14   | 255.255.255.0 |     10.10.0.3     |
|     S5     |      VLAN 11     |   10.11.0.5    | 255.255.255.0 |     10.11.0.11    |



## Таблица VLAN
| VLAN |        Имя        |               Назначение              |                    Используемые порты                 |
|:----:|:-----------------:|:-------------------------------------:|:-----------------------------------------------------:|
| 10/11| Network_Management|           Сетевые устройства          |                                                       |
| 20/21|   Infrastructure  |                Сервера                |                                                       |
|  30  |     Accounting    |              Бухгалтерия              |                       S2: Fa0/5-6                     |
|  40  |         HR        |                  HR                   |                       S2: Fa0/7-8                     |
|  50  |        SBER       |              Проект Сбер              |                       S4: Fa0/3-4                     |
|  60  |        VTB        |              Проект ВТБ               |                       S4: Fa0/5-6                     |
|  70  |        GPB        |              Проект ГПБ               |                       S4: Fa0/7-8                     |
|  100 |      IT_dept      |               IT отдел                |                       S2: Fa0/3-4                     |
|  999 |    Parking_Lot    |          Неиспользуемые порты         |S2/S4: Fa0/1-2, Fa0/9-10, Fa0/13-14, Fa0/16-24, Gi0/1-2|
|  999 |    Parking_Lot    |          Неиспользуемые порты         |S1/S3: Fa0/1-10, Fa0/16-24, Gi0/2 S5: Fa0/1-19         |
| 1000 |      Private      |             Нативный VLAN             |                                                       |


### Создание сабинтерфейсов
#### R1
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
R1(config)#int g0/2.11
R1(config-subif)#encapsulation dot1Q 11
R1(config-subif)#ip address 10.11.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/2.21
R1(config-subif)#encapsulation dot1Q 21
R1(config-subif)#ip address 10.21.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int Loopback1
R1(config-if)#ip address 172.16.1.1 255.255.255.0
R1(config-if)#exit
```
```
R1#show ip interface brief 
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0     unassigned      YES unset  up                    down 
GigabitEthernet0/1     unassigned      YES unset  up                    up 
GigabitEthernet0/1.10  10.10.0.1       YES manual up                    up 
GigabitEthernet0/1.20  10.20.0.1       YES manual up                    up 
GigabitEthernet0/1.30  10.30.0.1       YES manual up                    up 
GigabitEthernet0/1.40  10.40.0.1       YES manual up                    up 
GigabitEthernet0/1.50  10.50.0.1       YES manual up                    up 
GigabitEthernet0/1.60  10.60.0.1       YES manual up                    up 
GigabitEthernet0/1.70  10.70.0.1       YES manual up                    up 
GigabitEthernet0/1.100 10.100.0.1      YES manual up                    up 
GigabitEthernet0/1.1000unassigned      YES unset  up                    up 
GigabitEthernet0/2     unassigned      YES manual up                    up 
GigabitEthernet0/2.11  10.11.0.1       YES manual up                    up 
GigabitEthernet0/2.21  10.21.0.1       YES manual up                    up 
Loopback1              172.16.1.1      YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
```
#### R2
```
R2(config)#int g0/1.10
R2(config-subif)#encapsulation dot1Q 10
R2(config-subif)#ip address 10.10.0.2 255.255.255.0
R2(config-subif)#exit
R2(config)#int g0/1.20
R2(config-subif)#encapsulation dot1Q 20
R2(config-subif)#ip address 10.20.0.2 255.255.255.0
R2(config-subif)#exit
R2(config)#int g0/1.30
R2(config-subif)#encapsulation dot1Q 30
R2(config-subif)#ip address 10.30.0.2 255.255.255.0
R2(config-subif)#exit
R2(config)#int g0/1.40
R2(config-subif)#encapsulation dot1Q 40
R2(config-subif)#ip address 10.40.0.2 255.255.255.0
R2(config-subif)#exit
R2(config)#int g0/1.50
R2(config-subif)#encapsulation dot1Q 50
R2(config-subif)#ip address 10.50.0.2 255.255.255.0
R2(config-subif)#exit
R2(config)#int g0/1.60
R2(config-subif)#encapsulation dot1Q 60
R2(config-subif)#ip address 10.60.0.2 255.255.255.0
R2(config-subif)#exit
R2(config)#int g0/1.70
R2(config-subif)#encapsulation dot1Q 70
R2(config-subif)#ip address 10.70.0.2 255.255.255.0
R2(config-subif)#exit
R2(config)#int g0/1.100
R2(config-subif)#encapsulation dot1Q 100
R2(config-subif)#ip address 10.100.0.2 255.255.255.0
R2(config-subif)#exit
R2(config)#int g0/1.1000
R2(config-subif)#encapsulation dot1Q 1000 native
R2(config-subif)#exit
R2(config)#int g0/0.11
R2(config-subif)#encapsulation dot1Q 11
R2(config-subif)#ip address 10.11.0.2 255.255.255.0
R2(config-subif)#exit
R2(config)#int g0/0.21
R2(config-subif)#encapsulation dot1Q 21
R2(config-subif)#ip address 10.21.0.2 255.255.255.0
R2(config-subif)#exit
```
```
R2#show ip int brief 
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0     unassigned      YES unset  up                    up 
GigabitEthernet0/0.11  10.11.0.2       YES manual up                    up 
GigabitEthernet0/0.21  10.21.0.2       YES manual up                    up 
GigabitEthernet0/1     unassigned      YES unset  up                    up 
GigabitEthernet0/1.10  10.10.0.2       YES manual up                    up 
GigabitEthernet0/1.20  10.20.0.2       YES manual up                    up 
GigabitEthernet0/1.30  10.30.0.2       YES manual up                    up 
GigabitEthernet0/1.40  10.40.0.2       YES manual up                    up 
GigabitEthernet0/1.50  10.50.0.2       YES manual up                    up 
GigabitEthernet0/1.60  10.60.0.2       YES manual up                    up 
GigabitEthernet0/1.70  10.70.0.2       YES manual up                    up 
GigabitEthernet0/1.100 10.100.0.2      YES manual up                    up 
GigabitEthernet0/1.1000unassigned      YES unset  up                    up 
GigabitEthernet0/2     172.16.2.1      YES manual up                    up 
Vlan1                  unassigned      YES unset  administratively down down
```
### Настройка SSH на коммутаторах
```
R1(config)#ip domain-name luxtech.ru
R1(config)#crypto key generate rsa
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
R1(config)#ip ssh version 2
R1(config)#username admin secret 1qa@WS3ed
R1(config)#line vty 0 4
R1(config-line)#login local
R1(config-line)#transport input ssh
R1(config-line)#exit
```
```
R2(config)#ip domain-name luxtech.ru
R2(config)#crypto key generate rsa
How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
R2(config)#ip ssh version 2
R2(config)#username admin secret 1qa@WS3ed
R2(config)#line vty 0 4
R2(config-line)#login local
R2(config-line)#transport input ssh
R2(config-line)#exit
```
### Создание VLAN на коммутаторах S1-S4
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
### Создание VLAN на коммутаторе S5
```
S5(config)#vlan 11
S5(config-vlan)#name Network_Management2
S5(config-vlan)#vlan 21
S5(config-vlan)#name Infrastructunre2
```
```
S5(config)#int g0/2
S5(config-if)#switchport mode trunk
S5(config-if)#switchport trunk allowed vlan 11,21,1000
S5(config-if)#switchport trunk native vlan 1000
S5(config-if)#exit
S5(config)#int g0/1
S5(config-if)#switchport mode trunk 
S5(config-if)#switchport trunk allowed vlan 11,21,1000
S5(config-if)#switchport trunk native vlan 1000
S5(config-if)#exit
```
### Настройка SSH на коммутаторах
```
S1(config)#ip domain-name luxtech.ru
S1(config)#crypto key generate rsa
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
S1(config)#ip ssh version 2
S1(config)#username admin secret 1qa@WS3ed
S1(config)#line vty 0 4
S1(config-line)#login local
S1(config-line)#transport input ssh
S1(config-line)#exit
```
```
S2(config)#ip domain-name luxtech.ru
S2(config)#crypto key generate rsa
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
S2(config)#ip ssh version 2
S2(config)#username admin secret 1qa@WS3ed
S2(config)#line vty 0 4
S2(config-line)#login local
S2(config-line)#transport input ssh
S2(config-line)#exit
```
```
S3(config)#ip domain-name luxtech.ru
S3(config)#crypto key generate rsa
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
S3(config)#ip ssh version 2
S3(config)#username admin secret 1qa@WS3ed
S3(config)#line vty 0 4
S3(config-line)#login local
S3(config-line)#transport input ssh
S3(config-line)#exit
```
```
S4(config)#ip domain-name luxtech.ru
S4(config)#crypto key generate rsa
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
S4(config)#ip ssh version 2
S4(config)#username admin secret 1qa@WS3ed
S4(config)#line vty 0 4
S4(config-line)#login local
S4(config-line)#transport input ssh
S4(config-line)#exit

```
```
S5(config)#ip domain-name luxtech.ru
S5(config)#crypto key generate rsa
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
S5(config)#ip ssh version 2
S5(config)#username admin secret 1qa@WS3ed
S5(config)#line vty 0 4
S5(config-line)#login local
S5(config-line)#transport input ssh
S5(config-line)#exit
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
#### S5
```
S5(config)#int range fa0/1-19
S5(config-if-range)#switchport mode access 
S5(config-if-range)#switchport access vlan 999
S5(config-if-range)#shutdown
S5(config-if-range)#exit
```
### Настойка Etherchannel
#### S1
```
S1(config)#int vlan 10
S1(config-if)#ip address 10.10.0.11 255.255.255.0
S1(config-if)#exit
S1(config)#ip default-gateway 10.10.0.3
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
#### S5
```
S5(config)#int vlan 11
S5(config-if)#ip address 10.11.0.5 255.255.255.0
S5(config-if)#exit
S5(config)#ip default-gateway 10.11.0.1
```

### Настойка SPT
```
S1(config)#spanning-tree vlan 10,20,30,40,100,1000 root primary
S1(config)#spanning-tree vlan 50,60,70 root secondary
S1(config)#spanning-tree mode rapid-pvst 
S3(config)#spanning-tree vlan 50,60,70 root primary
S3(config)#spanning-tree vlan 10,20,30,40,100,1000 root secondary
S3(config)#spanning-tree mode rapid-pvst
S2(config)#spanning-tree mode rapid-pvst
S4(config)#spanning-tree mode rapid-pvst 
```
#### Настройка граничных портов
```
S2(config)#int range fa0/3-8
S2(config-if-range)#shutdown
S2(config-if-range)#switchport mode access
S2(config-if-range)#spanning-tree portfast
S2(config-if-range)#spanning-tree bpduguard enable 
S2(config-if-range)#no shutdown 
```
```
S4(config)#int range fa0/3-8
S4(config-if-range)#shutdown
S4(config-if-range)#switchport mode access
S4(config-if-range)#spanning-tree portfast
S4(config-if-range)#spanning-tree bpduguard enable 
S4(config-if-range)#no shutdown 
```
### Настройка DHCP
#### R1
```
R1(config)#ip dhcp pool R1_Accounting_vlan
R1(dhcp-config)#domain-name luxtech.ru
R1(dhcp-config)#network 10.30.0.0 255.255.255.0
R1(dhcp-config)#default-router 10.30.0.3
R1(dhcp-config)#dns-server 10.21.0.3
R1(dhcp-config)#ip dhcp excluded-address 10.30.0.1 10.30.0.3
R1(dhcp-config)#ip dhcp excluded-address 10.30.0.128 10.30.0.254
R1(config)#
R1(config)#ip dhcp pool R1_HR_vlan
R1(dhcp-config)#domain-name luxtech.ru
R1(dhcp-config)#network 10.40.0.0 255.255.255.0
R1(dhcp-config)#default-router 10.40.0.3
R1(dhcp-config)#dns-server 10.21.0.3
R1(dhcp-config)#ip dhcp excluded-address 10.40.0.1 10.40.0.3
R1(dhcp-config)#ip dhcp excluded-address 10.40.0.128 10.40.0.254
R1(config)#
R1(config)#ip dhcp pool R1_SBER_vlan
R1(dhcp-config)#domain-name luxtech.ru
R1(dhcp-config)#network 10.50.0.0 255.255.255.0
R1(dhcp-config)#default-router 10.50.0.3
R1(dhcp-config)#dns-server 10.21.0.3
R1(dhcp-config)#ip dhcp excluded-address 10.50.0.1 10.50.0.3
R1(dhcp-config)#ip dhcp excluded-address 10.50.0.128 10.50.0.254
R1(config)#
R1(config)#ip dhcp pool R1_VTB_vlan
R1(dhcp-config)#domain-name luxtech.ru
R1(dhcp-config)#network 10.60.0.0 255.255.255.0
R1(dhcp-config)#default-router 10.60.0.3
R1(dhcp-config)#dns-server 10.21.0.3
R1(dhcp-config)#ip dhcp excluded-address 10.60.0.1 10.60.0.3
R1(dhcp-config)#ip dhcp excluded-address 10.60.0.128 10.60.0.254
R1(config)#
R1(config)#ip dhcp pool R1_GPB_vlan
R1(dhcp-config)#domain-name luxtech.ru
R1(dhcp-config)#network 10.70.0.0 255.255.255.0
R1(dhcp-config)#default-router 10.70.0.3
R1(dhcp-config)#dns-server 10.21.0.3
R1(dhcp-config)#ip dhcp excluded-address 10.70.0.1 10.70.0.3
R1(dhcp-config)#ip dhcp excluded-address 10.70.0.128 10.70.0.254
R1(config)#
R1(config)#ip dhcp pool R1_IT_vlan
R1(dhcp-config)#domain-name luxtech.ru
R1(dhcp-config)#network 10.100.0.0 255.255.255.0
R1(dhcp-config)#default-router 10.100.0.3
R1(dhcp-config)#dns-server 10.21.0.3
R1(dhcp-config)#ip dhcp excluded-address 10.100.0.1 10.100.0.3
R1(dhcp-config)#ip dhcp excluded-address 10.100.0.128 10.100.0.254
R1(config)#
```
#### R2
```
R2(config)#ip dhcp pool R2_Accounting_vlan
R2(dhcp-config)#domain-name luxtech.ru
R2(dhcp-config)#network 10.30.0.0 255.255.255.0
R2(dhcp-config)#default-router 10.30.0.3
R2(dhcp-config)#dns-server 10.21.0.3
R2(dhcp-config)#ip dhcp excluded-address 10.30.0.1 10.30.0.127
R2(config)#
R2(config)#ip dhcp pool R2_HR_vlan
R2(dhcp-config)#domain-name luxtech.ru
R2(dhcp-config)#network 10.40.0.0 255.255.255.0
R2(dhcp-config)#default-router 10.40.0.3
R2(dhcp-config)#dns-server 10.21.0.3
R2(dhcp-config)#ip dhcp excluded-address 10.40.0.1 10.40.0.127
R2(config)#
R2(config)#ip dhcp pool R2_SBER_vlan
R2(dhcp-config)#domain-name luxtech.ru
R2(dhcp-config)#network 10.50.0.0 255.255.255.0
R2(dhcp-config)#default-router 10.50.0.3
R2(dhcp-config)#dns-server 10.21.0.3
R2(dhcp-config)#ip dhcp excluded-address 10.50.0.1 10.50.0.127
R2(config)#
R2(config)#ip dhcp pool R2_VTB_vlan
R2(dhcp-config)#domain-name luxtech.ru
R2(dhcp-config)#network 10.60.0.0 255.255.255.0
R2(dhcp-config)#default-router 10.60.0.3
R2(dhcp-config)#dns-server 10.21.0.3
R2(dhcp-config)#ip dhcp excluded-address 10.60.0.1 10.60.0.127
R2(config)#
R2(config)#ip dhcp pool R2_GPB_vlan
R2(dhcp-config)#domain-name luxtech.ru
R2(dhcp-config)#network 10.70.0.0 255.255.255.0
R2(dhcp-config)#default-router 10.70.0.3
R2(dhcp-config)#dns-server 10.21.0.3
R2(dhcp-config)#ip dhcp excluded-address 10.70.0.1 10.70.0.127
R2(config)#
R2(config)#ip dhcp pool R2_IT_vlan
R2(dhcp-config)#domain-name luxtech.ru
R2(dhcp-config)#network 10.100.0.0 255.255.255.0
R2(dhcp-config)#default-router 10.100.0.2
R2(dhcp-config)#dns-server 10.21.0.3
R2(dhcp-config)#ip dhcp excluded-address 10.100.0.1 10.100.0.127
R2(config)#

```
### Настройка маршрутизации между R1 и R2
```
R1(config)#ip route 172.16.2.1 255.255.255.255 10.10.0.2 - маршрут до g0/2 на R2
R1(config)#ip route 172.16.2.3 255.255.255.255 10.10.0.2 - маршрут до ISP через R2

R2(config)#ip route 10.21.0.0 255.255.255.0 10.10.0.1 - маршрут до 21 подсети на R1
```
### Настройка HSRP
#### Таблица HSRP

| VLAN |        HSRP       |
|:----:|:-----------------:|
|  10  |     10.10.0.3     |
|  11  |     10.11.0.11    |
|  20  |     10.20.0.3     |
|  21  |     10.21.0.11    |
|  30  |     10.30.0.3     |
|  40  |     10.40.0.3     |
|  50  |     10.50.0.3     |
|  60  |     10.60.0.3     |
|  70  |     10.70.0.3     |
|  100 |     10.100.0.3    |

#### R1
```
R1(config)#int g0/1.10
R1(config-subif)#standby version 2
R1(config-subif)#standby 10 ip 10.10.0.3
R1(config-subif)#standby 10 priority 150
R1(config-subif)#standby 10 preempt
R1(config-subif)#exit
R1(config)#int g0/1.20
R1(config-subif)#standby version 2
R1(config-subif)#standby 20 ip 10.20.0.3
R1(config-subif)#standby 20 priority 150
R1(config-subif)#standby 20 preempt
R1(config-subif)#exit
R1(config)#int g0/1.30
R1(config-subif)#standby version 2
R1(config-subif)#standby 30 ip 10.30.0.3
R1(config-subif)#standby 30 priority 150
R1(config-subif)#standby 30 preempt 
R1(config-subif)#exit
R1(config)#int g0/1.40
R1(config-subif)#standby version 2
R1(config-subif)#standby 40 ip 10.40.0.3
R1(config-subif)#standby 40 priority 150
R1(config-subif)#standby 40 preempt 
R1(config-subif)#exit
R1(config)#int g0/1.50
R1(config-subif)#standby version 2
R1(config-subif)#standby 50 ip 10.50.0.3
R1(config-subif)#standby 50 priority 150
R1(config-subif)#standby 50 preempt 
R1(config-subif)#exit
R1(config)#int g0/1.60
R1(config-subif)#standby version 2
R1(config-subif)#standby 60 ip 10.60.0.3
R1(config-subif)#standby 60 priority 150
R1(config-subif)#standby 60 preempt 
R1(config-subif)#exit
R1(config)#int g0/1.70
R1(config-subif)#standby version 2
R1(config-subif)#standby 70 ip 10.70.0.3
R1(config-subif)#standby 70 priority 150
R1(config-subif)#standby 70 preempt
R1(config-subif)#exit
R1(config)#int g0/1.100
R1(config-subif)#standby version 2
R1(config-subif)#standby 100 ip 10.100.0.3
R1(config-subif)#standby 100 priority 150
R1(config-subif)#standby 100 preempt
R1(config-subif)#exit
R1(config)#int g0/2.11
R1(config-subif)#standby version 2
R1(config-subif)#standby 11 ip 10.11.0.11
R1(config-subif)#standby 11 priority 150
R1(config-subif)#standby 11 preempt
R1(config-subif)#exit
R1(config)#int g0/2.21
R1(config-subif)#standby version 2
R1(config-subif)#standby 21 ip 10.21.0.11
R1(config-subif)#standby 21 priority 150
R1(config-subif)#standby 21 preempt 
R1(config-subif)#exit

```
#### R2
```
R2(config)#int gi0/1.10
R2(config-subif)#standby version 2
R2(config-subif)#standby 10 ip 10.10.0.3
R2(config-subif)#exit
R2(config)#int gi0/1.20
R2(config-subif)#standby version 2
R2(config-subif)#standby 20 ip 10.20.0.3
R2(config-subif)#exit
R2(config)#int gi0/1.30
R2(config-subif)#standby version 2
R2(config-subif)#standby 30 ip 10.30.0.3
R2(config-subif)#exit
R2(config)#int gi0/1.40
R2(config-subif)#standby version 2
R2(config-subif)#standby 40 ip 10.40.0.3
R2(config-subif)#exit
R2(config)#int gi0/1.50
R2(config-subif)#standby version 2
R2(config-subif)#standby 50 ip 10.50.0.3
R2(config-subif)#exit
R2(config)#int gi0/1.60
R2(config-subif)#standby version 2
R2(config-subif)#standby 60 ip 10.60.0.3
R2(config-subif)#exit
R2(config)#int gi0/1.70
R2(config-subif)#standby version 2
R2(config-subif)#standby 70 ip 10.70.0.3
R2(config-subif)#exit
R2(config)#int gi0/1.100
R2(config-subif)#standby version 2
R2(config-subif)#standby 100 ip 10.100.0.3
R2(config-subif)#exit
R2(config)#int g0/0.11
R2(config-subif)#standby version 2
R2(config-subif)#standby 11 ip 10.11.0.11
R2(config-subif)#exit
R2(config)#int g0/0.21
R2(config-subif)#standby version 2
R2(config-subif)#standby 21 ip 10.21.0.11
R2(config-subif)#exit


```
### Настройка NAT
#### R1
```
R1(config)#access-list 1 permit 10.0.0.0 0.255.255.255
R1(config)#ip nat pool PUBLIC_ACCESS 172.16.1.7 172.16.1.9 netmask 255.255.255.0
R1(config)#ip nat inside source list 1 pool PUBLIC_ACCESS
R1(config)#int g0/0
R1(config-if)#ip address 172.16.1.1 255.255.255.0
R1(config-if)#ip nat outside 
R1(config-if)#exit
R1(config)#int g0/1.10
R1(config-subif)#ip nat inside
R1(config-subif)#exit
R1(config)#int g0/1.20
R1(config-subif)#ip nat inside
R1(config-subif)#exit
R1(config)#int g0/1.30
R1(config-subif)#ip nat inside
R1(config-subif)#exit
R1(config)#int g0/1.40
R1(config-subif)#ip nat inside
R1(config-subif)#exit
R1(config)#int g0/1.50
R1(config-subif)#ip nat inside
R1(config-subif)#exit
R1(config)#int g0/1.60
R1(config-subif)#ip nat inside
R1(config-subif)#exit
R1(config)#int g0/1.70
R1(config-subif)#ip nat inside
R1(config-subif)#exit
R1(config)#int g0/1.100
R1(config-subif)#ip nat inside
R1(config-subif)#exit
R1(config)#int g0/2.11
R1(config-subif)#ip nat inside
R1(config-subif)#exit
R1(config)#int g0/2.21
R1(config-subif)#ip nat inside
R1(config-subif)#exit
```
#### R2
```
R2(config)#access-list 1 permit 10.0.0.0 0.255.255.255
R2(config)#ip nat pool PUBLIC_ACCESS 172.16.2.7 172.16.2.9 netmask 255.255.255.0
R2(config)#ip nat inside source list 1 pool PUBLIC_ACCESS
R2(config)#int g0/2
R2(config-if)#ip ad
R2(config-if)#ip address 172.16.2.1 255.255.255.0
R2(config-if)#ip nat outside
R2(config-if)#exit
R2(config)#int g0/1.10
R2(config-subif)#ip nat inside
R2(config-subif)#exit
R2(config)#int g0/1.20
R2(config-subif)#ip nat inside
R2(config-subif)#exit
R2(config)#int g0/1.30
R2(config-subif)#ip nat inside
R2(config-subif)#exit
R2(config)#int g0/1.40
R2(config-subif)#ip nat inside
R2(config-subif)#exit
R2(config)#int g0/1.50
R2(config-subif)#ip nat inside
R2(config-subif)#exit
R2(config)#int g0/1.60
R2(config-subif)#ip nat inside
R2(config-subif)#exit
R2(config)#int g0/1.70
R2(config-subif)#ip nat inside
R2(config-subif)#exit
R2(config)#int g0/1.100
R2(config-subif)#ip nat inside
R2(config-subif)#exit
```
### ACL
```
Требования по ограничению доступа:
1. Доступ к сетевым устройствам и компьютерам пользователей по 22 и 3389 разрешен только из IT VLAN.
2. Доступ к web серверам проектов по 80 и 443 разрешен только из IT VLAN или VLAN соответствующего проекта. Доступ к Homepage есть у всех.
3. Доступ к серверу 1C-Web по 80 и 443  портам есть только из VLAN Accounting и IT.
```
#### R1
```
R1(config)#ip access-list extended 110
R1(config-ext-nacl)#remark Deny SSH to computers and network devices
R1(config-ext-nacl)#deny tcp 10.0.0.0 0.255.255.255 10.0.0.0 0.255.255.255 eq 22
R1(config-ext-nacl)#deny tcp 10.0.0.0 0.255.255.255 10.0.0.0 0.255.255.255 eq 3389
R1(config-ext-nacl)#deny tcp 10.0.0.0 0.255.255.255 172.16.0.0 0.0.255.255 eq 22
R1(config-ext-nacl)#remark Deny HTTP/HTTPS
R1(config-ext-nacl)#deny tcp 10.30.0.0 0.0.255.255 10.21.0.5 0.0.0.0 eq 80
R1(config-ext-nacl)#deny tcp 10.30.0.0 0.0.255.255 10.21.0.5 0.0.0.0 eq 443
R1(config-ext-nacl)#deny tcp 10.30.0.0 0.0.255.255 10.21.0.6 0.0.0.0 eq 80
R1(config-ext-nacl)#deny tcp 10.30.0.0 0.0.255.255 10.21.0.6 0.0.0.0 eq 443
R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.255.255 10.21.0.5 0.0.0.0 eq 80
R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.255.255 10.21.0.5 0.0.0.0 eq 443
R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.255.255 10.21.0.6 0.0.0.0 eq 80
R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.255.255 10.21.0.6 0.0.0.0 eq 443
R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.255.255 10.21.0.10 0.0.0.0 eq 80
R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.255.255 10.21.0.10 0.0.0.0 eq 443
R1(config-ext-nacl)#deny tcp 10.50.0.0 0.0.255.255 10.21.0.6 0.0.0.0 eq 80
R1(config-ext-nacl)#deny tcp 10.50.0.0 0.0.255.255 10.21.0.6 0.0.0.0 eq 443
R1(config-ext-nacl)#deny tcp 10.50.0.0 0.0.255.255 10.21.0.10 0.0.0.0 eq 80
R1(config-ext-nacl)#deny tcp 10.50.0.0 0.0.255.255 10.21.0.10 0.0.0.0 eq 443
R1(config-ext-nacl)#deny tcp 10.60.0.0 0.0.255.255 10.21.0.5 0.0.0.0 eq 80
R1(config-ext-nacl)#deny tcp 10.60.0.0 0.0.255.255 10.21.0.5 0.0.0.0 eq 443
R1(config-ext-nacl)#deny tcp 10.60.0.0 0.0.255.255 10.21.0.10 0.0.0.0 eq 80
R1(config-ext-nacl)#deny tcp 10.60.0.0 0.0.255.255 10.21.0.10 0.0.0.0 eq 443
R1(config-ext-nacl)#deny tcp 10.70.0.0 0.0.255.255 10.21.0.5 0.0.0.0 eq 80
R1(config-ext-nacl)#deny tcp 10.70.0.0 0.0.255.255 10.21.0.5 0.0.0.0 eq 443
R1(config-ext-nacl)#deny tcp 10.70.0.0 0.0.255.255 10.21.0.6 0.0.0.0 eq 80
R1(config-ext-nacl)#deny tcp 10.70.0.0 0.0.255.255 10.21.0.6 0.0.0.0 eq 443
R1(config-ext-nacl)#deny tcp 10.70.0.0 0.0.255.255 10.21.0.10 0.0.0.0 eq 80
R1(config-ext-nacl)#deny tcp 10.70.0.0 0.0.255.255 10.21.0.10 0.0.0.0 eq 443
R1(config-ext-nacl)#permit ip any any
```
```
R1(config)#int g0/1.30
R1(config-subif)#ip access-group 110 in
R1(config-subif)#exit
R1(config)#int g0/1.40
R1(config-subif)#ip access-group 110 in
R1(config-subif)#exit
R1(config)#int g0/1.50
R1(config-subif)#ip access-group 110 in
R1(config-subif)#exit
R1(config)#int g0/1.60
R1(config-subif)#ip access-group 110 in
R1(config-subif)#exit
R1(config)#int g0/1.70
R1(config-subif)#ip access-group 110 in
R1(config-subif)#exit
R1(config)#int g0/1.100
R1(config-subif)#ip access-group 110 in
R1(config-subif)#exit
```
#### R2
```
R2(config)#ip access-list extended 110
R2(config-ext-nacl)#remark Deny SSH to computers and network devices
R2(config-ext-nacl)#deny tcp 10.0.0.0 0.255.255.255 10.0.0.0 0.255.255.255 eq 22
R2(config-ext-nacl)#deny tcp 10.0.0.0 0.255.255.255 10.0.0.0 0.255.255.255 eq 3389
R2(config-ext-nacl)#deny tcp 10.0.0.0 0.255.255.255 172.16.0.0 0.0.255.255 eq 22
R2(config-ext-nacl)#remark Deny HTTP/HTTPS
R2(config-ext-nacl)#deny tcp 10.30.0.0 0.0.255.255 10.21.0.5 0.0.0.0 eq 80
R2(config-ext-nacl)#deny tcp 10.30.0.0 0.0.255.255 10.21.0.5 0.0.0.0 eq 443
R2(config-ext-nacl)#deny tcp 10.30.0.0 0.0.255.255 10.21.0.6 0.0.0.0 eq 80
R2(config-ext-nacl)#deny tcp 10.30.0.0 0.0.255.255 10.21.0.6 0.0.0.0 eq 443
R2(config-ext-nacl)#deny tcp 10.40.0.0 0.0.255.255 10.21.0.5 0.0.0.0 eq 80
R2(config-ext-nacl)#deny tcp 10.40.0.0 0.0.255.255 10.21.0.5 0.0.0.0 eq 443
R2(config-ext-nacl)#deny tcp 10.40.0.0 0.0.255.255 10.21.0.6 0.0.0.0 eq 80
R2(config-ext-nacl)#deny tcp 10.40.0.0 0.0.255.255 10.21.0.6 0.0.0.0 eq 443
R2(config-ext-nacl)#deny tcp 10.40.0.0 0.0.255.255 10.21.0.10 0.0.0.0 eq 80
R2(config-ext-nacl)#deny tcp 10.40.0.0 0.0.255.255 10.21.0.10 0.0.0.0 eq 443
R2(config-ext-nacl)#deny tcp 10.50.0.0 0.0.255.255 10.21.0.6 0.0.0.0 eq 80
R2(config-ext-nacl)#deny tcp 10.50.0.0 0.0.255.255 10.21.0.6 0.0.0.0 eq 443
R2(config-ext-nacl)#deny tcp 10.50.0.0 0.0.255.255 10.21.0.10 0.0.0.0 eq 80
R2(config-ext-nacl)#deny tcp 10.50.0.0 0.0.255.255 10.21.0.10 0.0.0.0 eq 443
R2(config-ext-nacl)#deny tcp 10.60.0.0 0.0.255.255 10.21.0.5 0.0.0.0 eq 80
R2(config-ext-nacl)#deny tcp 10.60.0.0 0.0.255.255 10.21.0.5 0.0.0.0 eq 443
R2(config-ext-nacl)#deny tcp 10.60.0.0 0.0.255.255 10.21.0.10 0.0.0.0 eq 80
R2(config-ext-nacl)#deny tcp 10.60.0.0 0.0.255.255 10.21.0.10 0.0.0.0 eq 443
R2(config-ext-nacl)#deny tcp 10.70.0.0 0.0.255.255 10.21.0.5 0.0.0.0 eq 80
R2(config-ext-nacl)#deny tcp 10.70.0.0 0.0.255.255 10.21.0.5 0.0.0.0 eq 443
R2(config-ext-nacl)#deny tcp 10.70.0.0 0.0.255.255 10.21.0.6 0.0.0.0 eq 80
R2(config-ext-nacl)#deny tcp 10.70.0.0 0.0.255.255 10.21.0.6 0.0.0.0 eq 443
R2(config-ext-nacl)#deny tcp 10.70.0.0 0.0.255.255 10.21.0.10 0.0.0.0 eq 80
R2(config-ext-nacl)#deny tcp 10.70.0.0 0.0.255.255 10.21.0.10 0.0.0.0 eq 443
R2(config-ext-nacl)#permit ip any any
```
```
R2(config)#int g0/1.30
R2(config-subif)#ip access-group 110 in
R2(config-subif)#exit
R2(config)#int g0/1.40
R2(config-subif)#ip access-group 110 in
R2(config-subif)#exit
R2(config)#int g0/1.50
R2(config-subif)#ip access-group 110 in
R2(config-subif)#exit
R2(config)#int g0/1.60
R2(config-subif)#ip access-group 110 in
R2(config-subif)#exit
R2(config)#int g0/1.70
R2(config-subif)#ip access-group 110 in
R2(config-subif)#exit
R2(config)#int g0/1.100
R2(config-subif)#ip access-group 110 in
R2(config-subif)#exit
```
