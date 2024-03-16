# Лабораторная работа. Развертывание коммутируемой сети с резервными каналами
## Топология
![alt text](https://github.com/V1RaJ97/OTUS-NE/blob/e1087a19b35e65d7d60702d9a9711ab30e438e6e/Labs/Lab07/%D0%A1%D1%85%D0%B5%D0%BC%D0%B0%20%D1%81%D0%B5%D1%82%D0%B8.png)

## Таблица адресации
| Устройство |  Интерфейс  |   IP-адрес  | Маска подсети |
|:----------:|:-----------:|:-----------:|:-------------:|
|     S1     |    VLAN 1   | 192.168.1.1 | 255.255.255.0 |
|     S2     |    VLAN 1   | 192.168.1.2 | 255.255.255.0 |
|     S3     |    VLAN 1   | 192.168.1.3 | 255.255.255.0 |

## Задачи
1. Создание сети и настройка основных параметров устройства
2. Выбор корневого моста
3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов
4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов

## Часть 1:	Создание сети и настройка основных параметров устройства
### Настройка базовых параметров каждого коммутатора.
```
Switch(config)#hostname S1
S1(config)#enable secret class
S1(config)#service password-encryption
S1(config)#banner motd 1 Unauthorized access is strictly prohibited 1
S1(config)#no ip domain-lookup
```
```
S1(config)#line console 0
S1(config-line)#password cisco
S1(config-line)#logging synchronous
S1(config-line)#login
S1(config-line)#end
```
```
S1(config)#line vty 0 4
S1(config-line)#logging synchronous 
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#end
```
```
S1(config)#int VLAN 1
S1(config-if)#no shutdown
S1(config-if)#ip address 192.168.1.1 255.255.255.0
S1(config-if)#end
```
