#  Лабораторная работа - Реализация DHCPv4   

###  Задание:  

 1. Создание сети и настройка основных параметров устройства  
 2. Настройка и проверка двух серверов DHCPv4 на R1  
 3. Настройка и проверка DHCP-ретрансляции на R2 

 
    
  
###  Дано:
#### Таблица адресации:
| Устройство   | Интерфейс   | IP-адресс   |Маска подсети     | Шлюз по умолчанию | 
|-------------:|:------------|:------------|:-----------------|:------------------|  
| R1           | G0/0/0      | 10.0.0.1    | 255.255.255.252  |        -          |   
|              | G0/0/1      | ---------   | ---------------  |        -          |   
|              | G0/0/1.100  | 192.168.1.1 | 255.255.255.192  |        -          |   
|              | G0/0/1.200  | 192.168.1.65| 255.255.255.254  |        -          |   
|              | G0/0/1.1000 | --------    |---------------   |        -          |   
| R2           | G0/0/0      | 10.0.0.2    | 255.255.255.252  |        -          |   
|              | G0/0/1      | 192.168.1.97| 255.255.255.240  |        -          |   
| S1           | VLAN 200    |             | 255.255.255.192  |   192.168.1.65    |        
| S2           | VLAN 1      | 192.168.1.3 | 255.255.255.0    |                   |        
| PC-A         | NIC         | DHCP        | DHCP             |  DHCP             |        
| PC-B         | NIC         | DHCP        | DHCP             |  DHCP             |        

    
#### Таблица VLAN:
| VLAN  | Имя         | Назначенный интерфейс       |
|:------|:------------|:----------------------------|
|1      | Нет         | S2: F0/18                   | 
|200    | Admin       | S1: VLAN 200                | 
|999    | Parking_Lot | S1: F0/1-4, F0/7-24, G0/1-2 | 
|1000   |             | -                           | 

#### Топология:  
   ![Топология](image.png)  
  
###  Решение:  
 
###  1. Создание сети и настройка основных параметров устройства; 
  1. Создаем сеть согласно топологии.  
          
  2. Настрайваем параметры для устройств  

      [Получим результат S1;][def]  
      
      [Получим результат S2;][def1]  

      [Получим результат R1;][def2]  

      [Получим результат R2;][def3]  


      
  3. Настроим G0/0/1 на R2, затем G0/0/0 и статическую маршрутизацию для обоих маршрутизаторов.

            ip route 0.0.0.0 0.0.0.0 10.0.0.1
            
            аналогично на r1

  4. Убедимся что статическая маршрутизация работает с помощью пинга до адреса G0/0/1 R2 от R1.

            r2# ping 192.168.1.65

            Type escape sequence to abort.
            Sending 5, 100-byte ICMP Echos to 192.168.1.65, timeout is 2 seconds:
            !!!!!
            Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms

  5. Вручную настройте интерфейс S1 F0/5 в качестве транка 802.1Q.

            interface FastEthernet0/5  
            switchport trunk native vlan 1000  
            switchport trunk allowed vlan 100,200  
            switchport mode trunk  

   Какой IP-адрес был бы у ПК, если бы он был подключен к сети с помощью DHCP?
    
            В конфигурации DHCP на маршрутизаторе Cisco адрес шлюза (192.168.1.1) исключается из раздачи командой:
            ip dhcp excluded-address 192.168.1.1
            Поэтому первый адрес, который DHCP-сервер предложит клиенту — 192.168.1.2.

###  2. Настройка и проверка двух серверов DHCPv4 на R1  
  1. 	Настрайваем R1 с пулами DHCPv4 для двух поддерживаемых подсетей.  

            ip dhcp excluded-address 192.168.1.1 192.168.1.5  
            ip dhcp excluded-address 192.168.1.97 192.168.1.101  
            !  
            ip dhcp pool lan100  
            network 192.168.1.0 255.255.255.192  
            default-router 192.168.1.1  
            domain-name CCNA-lab.com  
            lease 2 12 30 (Cisco PT Нету данной команды)

            ip dhcp pool client_lan  
            network 192.168.1.96 255.255.255.240  
            default-router 192.168.1.97  
            domain-name CCNA-lab.com  
            lease 2 12 30

  2. Проверка конфигурации сервера DHCPv4
                  R1#show ip dhcp pool 

                  Pool lan100 :
                  Utilization mark (high/low)    : 100 / 0
                  Subnet size (first/next)       : 0 / 0 
                  Total addresses                : 62
                  Leased addresses               : 1
                  Excluded addresses             : 2
                  Pending event                  : none

                  1 subnet is currently in the pool
                  Current index        IP address range                    Leased/Excluded/Total
                  192.168.1.1          192.168.1.1      - 192.168.1.62      1    / 2     / 62

                  Pool client_lan :
                  Utilization mark (high/low)    : 100 / 0
                  Subnet size (first/next)       : 0 / 0 
                  Total addresses                : 14
                  Leased addresses               : 1
                  Excluded addresses             : 2
                  Pending event                  : none

                  1 subnet is currently in the pool
                  Current index        IP address range                    Leased/Excluded/Total
                  192.168.1.97         192.168.1.97     - 192.168.1.110     1    / 2     / 14
                  R1#

                  R1#show ip dhcp binding 
                  IP address       Client-ID/              Lease expiration        Type
                              Hardware address
                  192.168.1.6      000C.85BE.50A5           --                     Automatic
      
  3. Получаем IP-адрес от DHCP на PC-A

            C:\>ipconfig

            FastEthernet0 Connection:(default port)

            Connection-specific DNS Suffix..: CCNA-lab.com
            Link-local IPv6 Address.........: FE80::20C:85FF:FEBE:50A5
            IPv6 Address....................: ::
            IPv4 Address....................: 192.168.1.6
            Subnet Mask.....................: 255.255.255.192
            Default Gateway.................: ::
                                                192.168.1.1

  
###  3. Настройка и проверка DHCP-ретрансляции на R2;  

   1. Настройка R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/0/1     
          
            ip helper-address 10.0.0.1 

   2. Получаем IP-адрес от DHCP на PC-B

            C:\>ipconfig

            FastEthernet0 Connection:(default port)

            Connection-specific DNS Suffix..: CCNA-lab.com
            Link-local IPv6 Address.........: FE80::2E0:B0FF:FED6:E42E
            IPv6 Address....................: ::
            IPv4 Address....................: 192.168.1.102
            Subnet Mask.....................: 255.255.255.240
            Default Gateway.................: ::
                                                192.168.1.97


     
[def]: conf/base_conf_S1.md   
[def1]: conf/base_conf_S2.md    
[def2]: conf/base_conf_R1.md   
[def3]: conf/base_conf_R2.md   