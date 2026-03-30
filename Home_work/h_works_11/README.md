#  Лабораторная работа. Настройка и проверка расширенных списков контроля доступа. 

###  Задание:  

 1. Создание сети и настройка основных параметров устройства.   
 2. Настройка и проверка списков расширенного контроля доступа.       


 
    
  
###  Дано:
#### Таблица адресации:
| Устройство   | Интерфейс   | IP-адрес       | Маска подсети | Шлюз по умолчанию |  
|-------------:|:------------|:---------------|:--------------|:------------------|    
| R1           | G0/0/1      |      ---       |       ---     |    ---            |     
|              | G0/0/1.20   | 10.20.0.1      | 255.255.255.0 |                   |     
|              | G0/0/1.30   | 10.30.0.1      | 255.255.255.0 |                   |     
|              | G0/0/1.40   | 10.49.0.1      | 255.255.255.0 |                   |     
|              | G0/0/1.1000 |      ---       |      ---      |                   |     
|              | Loopback    | 172.16.1.1     | 255.255.255.0 |                   |   
| R2           | G0/0/1      | 10.20.0.4      | 255.255.255.0 |                   |    
| S1           | VLAN 20     | 10.20.0.2      | 255.255.255.0 | 10.20.0.1         |    
| S2           | VLAN 20     | 10.20.0.3      | 255.255.255.0 | 10.20.0.1         |    
| PC-A         | NIC         | 10.30.0.10     | 255.255.255.0 | 10.30.0.1         |    
| PC-B         | NIC         | 10.40.0.10     | 255.255.255.0 | 10.40.0.1         |    

#### Таблица VLAN:
|   VLAN   |    Имя    |       Назначенный интерфейс        |  
|:---------|:----------|:-----------------------------------|
|    20    | Managment | S2: F0/5                           |
|    30    | Operations| S1: F0/6                           |
|    40    | Sales     | S2: F0/18                          |
|    999   | ParkingLot| S1: F0/2-4,F0/7-24,G0/1-2          |
|          |           | S2: F0/2-4,F0/6-17,F0/19-24,G0/1-2 |
|   1000   | Sobstvenay|      ---                           |
    
#### Топология:  
   ![Топология](image.png)  
  
###  Решение:  
 
###  1.Создание сети и настройка основных параметров устройства;   
  1. Создаем сеть согласно топологии.  
          
  2. Настрайваем параметры для устройств  

      [Получим результат S1;][def]  
      
      [Получим результат S2;][def1]  

      [Получим результат R1;][def2]  

      [Получим результат R2;][def3]

###  2. Настройка сетей VLAN на коммутаторах.      
   1.	Создайте сети VLAN на коммутаторах. 
        a. Настройка коммутатора S1.  
    
            S1(config)#vlan 20  
            S1(config-vlan)#name Managment  
            S1(config)#vlan 30  
            S1(config-vlan)#name Operations  
            S1(config)#vlan 40  
            S1(config-vlan)#name Sales  
            S1(config)#vlan 999  
            S1(config-vlan)#name ParkingLot  
            S1(config-vlan)#vlan 1000  
       
            S1(config)#interface FastEthernet 0/6    
            S1(config-if)#switchport mode access     
            S1(config-if)#switchport access vlan 30    
            S1(config)#interface range f0/2-4,f0/7-24,g0/1-2    
            S1(config-if-range)#switchport mode access     
            S1(config-if-range)#switchport access vlan 999    
            S1(config-if-range)#shutdown     
  
        b. Настройка коммутатора S2.   
    
            S2(config)#vlan 20  
            S2(config-vlan)#name Management  
            S2(config)#vlan 30  
            S2(config-vlan)#name Operations  
            S2(config)#vlan 40  
            S2(config-vlan)#name Sales  
            S2(config)#vlan 999  
            S2(config-vlan)#name ParkingLot  
            S2(config)#vlan 1000  
  
            S2(config)#interface fastEthernet 0/5  
            S2(config-if)#switchport mode access   
            S2(config-if)#switchport access vlan 20  
            S2(config)#interface f0/18  
            S2(config-if)#switchport mode access   
            S2(config-if)#switchport access vlan 40  
            S2(config)#interface range f0/2-4,f0/6-17,f0/19-24,g0/1-2  
            S2(config-if-range)#switchport mode access   
            S2(config-if-range)#switchport access vlan 999  
            S2(config-if-range)#shutdown   
  
    
###  3. Настройте транки (магистральные каналы).    
   1. Вручную настроим магистральный интерфейс F0/1.    
    
            S1(config)#interface fastEthernet 0/1  
            S1(config-if)#switchport mode trunk   
            S1(config-if)#switchport trunk native vlan 1000  
            S1(config-if)#switchport trunk allowed vlan 20,30,40  
  
            S2(config)#interface f0/1  
            S2(config-if)#switchport mode trunk   
            S2(config-if)#switchport trunk native vlan 1000  
            S2(config-if)#switchport trunk allowed vlan 20,30,40  
  
  
   2. Вручную настроим магистральный интерфейс F0/5 на коммутаторе S1..     
       
            S1(config)#interface f 0/5  
            S1(config-if)#switchport mode trunk   
            S1(config-if)#switchport trunk native vlan 1000  
            S1(config-if)#switchport trunk allowed vlan 20,30,40  

### 4. Настройте маршрутизацию.  
   1. Настройка маршрутизации между сетями VLAN на R1  
  
            R1(config)#interface gigabitEthernet 0/0/1.20  
            R1(config-subif)#encapsulation dot1Q 20  
            R1(config-subif)#ip address 10.20.0.1 255.255.255.0  
            R1(config-subif)#description Management  
            R1(config-subif)#exit   
            R1(config)#interface gigabitEthernet 0/0/1.30  
            R1(config-subif)#encapsulation dot1Q 30  
            R1(config-subif)#ip address 10.30.0.1 255.255.255.0  
            R1(config-subif)#description Operations  
            R1(config-subif)#exit   
            R1(config)#interface gigabitEthernet 0/0/1.40  
            R1(config-subif)#ip address 10.40.0.1 255.255.255.0  
            R1(config-subif)#encapsulation dot1Q 40  
            R1(config-subif)#ip address 10.40.0.1 255.255.255.0
            R1(config-subif)#description Sales    
            R1(config-subif)#exit  
            R1(config)#interface gigabitEthernet 0/0/1.1000  
            R1(config-subif)#encapsulation dot1Q 1000 native 
            R1(config-subif)#no ip address  
            R1(config-subif)#description native  
            R1(config-subif)#exit  

   2. Настроим интерфейс R2 g0/0/1 с использованием адреса из таблицы и маршрута по умолчанию с адресом следующего перехода 10.20.0.1

            R2(config)#interface gigabitEthernet 0/0/1
            R2(config-if)#ip address 10.20.0.4 255.255.255.0
            R2(config-if)#no shutdown 
            R2(config)#ip route 0.0.0.0 0.0.0.0 10.20.0.1
            R2(config)#exit

### 5. Настройте удаленный доступ.   
   1. Настроим все сетевые устройства для базовой поддержки SSH.   


            username SSHadmin secret $cisco123!
            ip domain-name ccna-lab.ru
            crypto key generate rsa 
            line vty 0 4
            login local
            transport input ssh 
            exit
 
  2. Включим защищенные веб-службы с проверкой подлинности на R1.  


### 5. Проверка подключения.   
   1. Выполним следующие тесты.  
  
            R1:  
            C:\>ping 10.40.0.10  
            Reply from 10.40.0.10: bytes=32 time<1ms TTL=127  
            C:\>ping 10.20.0.1  
            Reply from 10.20.0.1: bytes=32 time<1ms TTL=255  
  
            R2:  
            C:\>ping 10.30.0.10  
            Reply from 10.30.0.10: bytes=32 time<1ms TTL=127  
            C:\>ping 10.20.0.1  
            Pinging 10.20.0.1 with 32 bytes of data:  
            Reply from 10.20.0.1: bytes=32 time<1ms TTL=255  
            C:\>ping 172.16.1.1  
            Reply from 172.16.1.1: bytes=32 time<1ms TTL=255  
  
### 6. Часть 7. Настройка и проверка списков контроля доступа (ACL).   
   1. Политика №1: Сеть Sales не может использовать SSH в сети Management (но в  другие сети SSH разрешен).  

            R1(config)#ip access-list extended BLOCK_SALES_SSH  
            R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.25 eq 22  
            R1(config-ext-nacl)#permit ip any any   
            R1(config-ext-nacl)#exit  

   1. Политика №2: Сеть Sales не имеет доступа к IP-адресам в сети Management с помощью любого веб-протокола (HTTP/HTTPS). Сеть Sales также не имеет доступа к интерфейсам R1 с помощью любого веб-протокола. Разрешён весь другой веб-трафик (обратите внимание — Сеть Sales  может получить доступ к интерфейсу Loopback 1 на R1).   
  
            R1(config)#ip access-list extended BLOCK_SALES_WEB  
            R1(config-ext-nacl)#permit tcp 10.40.0.0 0.0.0.255 host 192.168.1.1 eq 80  
            R1(config-ext-nacl)#permit tcp 10.40.0.0 0.0.0.255 host 192.168.1.1 eq 443  
            R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 80  
            R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 443  
            R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.0.255 host 10.10.0.1 eq 80  
            R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.0.255 host 10.10.0.1 eq 443  
            R1(config-ext-nacl)#permit ip any any   
            R1(config-ext-nacl)#exit  
            R1(config)#interface GigabitEthernet0/0/1
            R1(config-if)#ip access-group BLOCK_SALES_WEB in
            R1(config-if)#exit


__________________________________дальше не делал
  
            R1(config)#interface loopback 1
            R1(config-if)#ip address 172.16.1.1 255.255.255.0
            R1(config-if)#no shutdown 
            R1(config-if)#exit
            R1(config)#ip route 0.0.0.0 0.0.0.0 l
            R1(config)#ip route 0.0.0.0 0.0.0.0 loopback 1
  
            R1#show ip ospf interface GigabitEthernet0/0/1  
  
            GigabitEthernet0/0/1 is up, line protocol is up  
            Internet address is 10.53.0.1/24, Area 0  
            Process ID 56, Router ID 1.1.1.1, Network Type >>>BROADCAST<<, Cost: 10  
            Transmit Delay is 1 sec, State DR, Priority >>>50<<<  
            Designated Router (ID) 1.1.1.1, Interface address 10.53.0.1  
            Backup Designated Router (ID) 2.2.2.2, Interface address 10.53.0.2  
            Timer intervals configured, >>>Hello 30<<<, >>>Dead 120<<<, Wait 120, Retransmit 5  
               Hello due in 00:00:12  
            Index 1/1, flood queue length 0  
            Next 0x0(0)/0x0(0)  
            Last flood scan length is 1, maximum is 1  
            Last flood scan time is 0 msec, maximum is 0 msec  
            Neighbor Count is 1, Adjacent neighbor count is 1  
               Adjacent with neighbor 2.2.2.2  (Backup Designated Router)  
            Suppress hello for 0 neighbor(s)  
  
            R1#show ip route ospf  
            O    192.168.1.0 [110/10] via 10.53.0.2, 00:08:00, GigabitEthernet0/0/1  
  
            R2#ping 172.16.1.1  
            Type escape sequence to abort.  
            Sending 5, 100-byte ICMP Echos to 172.16.1.1, timeout is 2 seconds:  
            !!!!!  
            Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/1 ms  
   
* Вопрос: Почему стоимость OSPF для маршрута по умолчанию отличается от стоимости OSPF в R1 для сети 192.168.1.0/24? 
   
         Маршрут по умолчанию:  
           * Маршрут по умолчанию не накапливают стоимость через OSPF-домен  
         Сеть 192.168.1.0/24:  
           * Стоимость накапливается по всем интерфейсам на пути  

[def]: conf/base_conf_S1.md   
[def1]: conf/base_conf_S2.md    
[def2]: conf/base_conf_R1.md   
[def3]: conf/base_conf_R2.md   
 