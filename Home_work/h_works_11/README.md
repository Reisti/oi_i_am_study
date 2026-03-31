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
   1. Выполним следующие тесты.(Из за ограничение функционала CPT добаляем web сервер с IP 10.20.0.2).   
  
|   От   |   Протокол   |   Назначение   | Результат |  
|:-------|:-------------|:---------------|:----------|  
| PC-A   | Ping         | 10.40.0.10     | Успех     |   
| PC-A   | Ping         | 10.20.0.1      | Успех     |   
| PC-B   | Ping         | 10.30.0.10     | Успех     |   
| PC-B   | Ping         | 10.20.0.1      | Успех     |   
| PC-B   | HTTPS        | 10.20.0.2      | Успех     |   
| PC-B   | SSH          | 10.20.0.1      | Успех     |   
| PC-B   | SSH          | 172.16.1.1     | Успех     |   

  
### 6. Настройка и проверка списков контроля доступа (ACL).   
   1. Сеть Sales не может использовать SSH в сети Management (но в  другие сети SSH разрешен).  

            R1(config)#ip access-list extended BLOCK_SALES_SSH  
            R1(config-ext-nacl)# 5 deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 22  
            R1(config-ext-nacl)#permit ip any any   
            R1(config-ext-nacl)#exit  

   2. Сеть Sales не имеет доступа к IP-адресам в сети Management с помощью любого веб-протокола (HTTP/HTTPS). Сеть Sales также не имеет доступа к интерфейсам R1 с помощью любого веб-протокола. Разрешён весь другой веб-трафик (обратите внимание — Сеть Sales  может получить доступ к интерфейсу Loopback 1 на R1).   
  
            R1(config)#ip access-list extended BLOCK_SALES_WEB  
            R1(config-ext-nacl)#permit tcp 10.40.0.0 0.0.0.255 host 172.16.1.1 eq 80  
            R1(config-ext-nacl)#permit tcp 10.40.0.0 0.0.0.255 host 172.16.1.1 eq 443  
            R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 80  
            R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 443  
            R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.0.255 host 10.10.0.1 eq 80  
            R1(config-ext-nacl)#deny tcp 10.40.0.0 0.0.0.255 host 10.10.0.1 eq 443  
            R1(config-ext-nacl)#permit ip any any   
            R1(config-ext-nacl)#exit  
            R1(config)#interface GigabitEthernet0/0/1  
            R1(config-if)#ip access-group BLOCK_SALES_WEB in  
            R1(config-if)#exit    

   3. Сеть Sales не может отправлять эхо-запросы ICMP в сети Operations или Management. Разрешены эхо-запросы ICMP к другим адресатам.  
  
            R1(config)# ip access-list extended SALES_POLICY  
            R1(config-ext-nacl)#deny icmp 10.40.0.0 0.0.0.255 10.30.0.0 0.0.0.255 echo  
            R1(config-ext-nacl)#deny icmp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 echo  
            R1(config-ext-nacl)# permit icmp 10.30.0.0 0.0.0.255 10.40.0.0 0.0.0.255 echo-reply  
            R1(config-ext-nacl)#permit icmp 10.20.0.0 0.0.0.255 10.40.0.0 0.0.0.255 echo-reply  
            R1(config-ext-nacl)#permit icmp any any  
            R1(config-ext-nacl)#permit ip any any  
            R1(config-ext-nacl)#end  
            R1(config)# interface GigabitEthernet0/0/1.40  
            R1(config-subif)# ip access-group SALES_POLICY in  
            R1(config-subif)# exit  
              
    
   3. Cеть Operations  не может отправлять ICMP эхозапросы в сеть Sales. Разрешены эхо-запросы ICMP к другим адресатам.   
  
            R1(config)#ip access-list extended BLOCK_OP_ICMP  
            R1(config-ext-nacl)#deny icmp 10.30.0.0 0.0.0.255 10.40.0.0 0.0.0.255 echo  
            R1(config-ext-nacl)#permit icmp 10.40.0.0 0.0.0.255 10.30.0.0 0.0.0.255 echo-reply  
            R1(config-ext-nacl)#permit icmp any any  
            R1(config-ext-nacl)#permit ip any any  
            R1(config-ext-nacl)#exit  
            R1(config)# interface GigabitEthernet0/0/1.30  
            R1(config-subif)# ip access-group BLOCK_OP_ICMP in  
            R1(config-subif)# exit  
  
### 7.Убедитесь, что политики безопасности применяются развернутыми списками доступа.

|   От   |   Протокол   |   Назначение   | Результат |  
|:-------|:-------------|:---------------|:----------|  
| PC-A   | Ping         | 10.40.0.10     | Сбой      |   
| PC-A   | Ping         | 10.20.0.1      | Успех     |   
| PC-B   | Ping         | 10.30.0.10     | Сбой      |   
| PC-B   | Ping         | 10.20.0.1      | Сбой      |   
| PC-B   | Ping         | 172.16.1.1     | Успех     |   
| PC-B   | HTTPS        | 10.20.0.2      | Сбой      |   
| PC-B   | SSH          | 10.20.0.1      | Сбой      |   
| PC-B   | SSH          | 172.16.1.1     | Успех     |  


[def]: conf/base_conf_S1.md   
[def1]: conf/base_conf_S2.md    
[def2]: conf/base_conf_R1.md   
[def3]: conf/base_conf_R2.md   
 