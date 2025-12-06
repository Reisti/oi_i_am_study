#  Базовая настройка коммутатора

###  Задание:

  1. Проверка конфигурации коммутатора по умолчанию;
  2. Создание сети и настройка основных параметров устройства;
    2.1. Настройте базовые параметры коммутатора.
    2.2. Настройте IP-адрес для ПК
  3. Проверка сетевых подключений
    3.1 Отобразите конфигурацию устройства.
    3.2 Протестируйте сквозное соединение, отправив эхо-запрос.
    3.3 Протестируйте возможности удаленного управления с    помощью Telnet
  

###  Дано:
#### Таблица адресации:
| Устройство    | Интерфейс    | IP-адресс          |
|--------------:|:-------------|-------------------:|
| S1            | Vlan 1       | 192.168.1.2        | 
|               |              | 255.255.255.0      | 
| PC-A          | NIC          | 192.168.1.10       | 
|               |              | 255.255.255.0      | 

#### Топология:

###  Решение:
 
###  1. Проверка конфигурации коммутатора по умолчанию.
  1.1. Подключаемся через консоль, и вводим команды
    enable
    show running-config
    Получим Результат

Switch#show running-config 
Building configuration...

Current configuration : 1080 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
no service password-encryption
!
hostname Switch
!
!
!
!
!
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1 ...

... interface FastEthernet0/24
!
interface GigabitEthernet0/1
!
interface GigabitEthernet0/2
!
interface Vlan1
no ip address
shutdown
!
!
!
!
line con 0
!
line vty 0 4
login
line vty 5 15
login
!
!
!
!
end
