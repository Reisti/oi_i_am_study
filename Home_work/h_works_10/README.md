#  Лабораторная работа. Настройка протокола OSPFv2 для одной области  

###  Задание:  

 1. Создание сети и настройка основных параметров устройства.   
 2. Настройка и проверка базовой работы протокола  OSPFv2 для одной области.   
 3. Оптимизация и проверка конфигурации OSPFv2 для одной области.    


 
    
  
###  Дано:
#### Таблица адресации:
| Устройство   | Интерфейс   | IP-адрес       | Маска подсети |
|-------------:|:------------|:---------------|:--------------|  
| R1           | G0/0/1      | 10.53.0.1      | 255.255.255.0 |    
|              | Loopback    | 172.16.1.1     | 255.255.255.0 |  
| R2           | G0/0/1      | 10.53.0.2      | 255.255.255.0 |  
|              | Loopback    | 192.168.1.1    | 255.255.255.0 |  
    
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

###  2. Настройка и проверка базовой работы протокола OSPFv2 для одной области.      
   1.	Настройваем адреса интерфейса и базового OSPFv2 на каждом маршрутизаторе.  
  
            R1(config)#router ospf 56   
            R1(config-router)#router-id 1.1.1.1   
            R1(config-router)#network 10.53.0.0 0.0.0.255 area 0  
     
      Проверка:  
  
            R1#show ip ospf neighbor  
            Neighbor ID Pri State Dead Time Address Interface  
            2.2.2.2 1 FULL/BDR 00:00:33 10.53.0.2 GigabitEthernet0/0/1  



            R2(config)#router ospf 56  
            R2(config-router)#router-id 2.2.2.2  
            R2(config-router)#network 10.53.0.0 0.0.0.255 area 0  
            R2(config-router)# network 192.168.1.0 0.0.0.255 area 0  
    
        Проверка:  
          
            R2#show ip ospf neighbor  
            Neighbor ID Pri State Dead Time Address Interface  
            1.1.1.1 1 FULL/DR 00:00:33 10.53.0.1 GigabitEthernet0/0/1  

   2. Проверка.    
  
            R1#show ip route ospf   
            O    192.168.1.0 [110/2] via 10.53.0.2, 00:00:01, GigabitEthernet0/0/1  
                
            R1#ping 192.168.1.1  
            Type escape sequence to abort.  
            Sending 5, 100-byte ICMP Echos to 192.168.1.1, timeout is 2 seconds:  
            !!!!!  
            Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/4 ms  
  
  
###  3. Оптимизация и проверка конфигурации OSPFv2 для одной области.    
   1. Настроим приоритет OSPF, и таймеры.    
  
            R1(config)# interface GigabitEthernet0/0/1  
            R1(config-if)# ip ospf priority 50  
            R1(config-if)# ip ospf hello-interval 30  
            R1(config-if)# ip ospf dead-interval 120  
  
            R2(config)# interface GigabitEthernet0/0/1  
            R2(config-if)# ip ospf hello-interval 30  
            R2(config-if)# ip ospf dead-interval 120  
           
   2. На R1 настройм статический маршрут.   
     
            R1(config)# ip route 0.0.0.0 0.0.0.0 Loopback1   
            R1(config)# router ospf 56   
            R1(config-router)# default-information originate  
  
        Проверка:  
            
            R2#show ip route ospf   
            O*E2 0.0.0.0/0 [110/1] via 10.53.0.1, 00:12:35, GigabitEthernet0/0/1  
  
   3. Добавим конфигурацию, необходимую для OSPF для обработки R2 Loopback 1 как сети точка-точка.  
  
            R2(config)# interface Loopback1  
            R2(config-if)# ip ospf network point-to-point  
            R2(config-if)# end  
            R2# clear ip ospf process  
              
        Проверка:  
          
            R1# show ip route ospf  
            O    192.168.1.0/24 [110/2] via 10.53.0.2, GigabitEthernet0/0/1  
  
   4. На R2 добавим конфигурацию, необходимую для предотвращения отправки объявлений OSPF в сеть Loopback 1.  
   
            R2(config)# router ospf 56  
            R2(config-router)# passive-interface Loopback1  

        Проверка:    
            
            R2#show ip ospf interface loopback 1  
  
            Loopback1 is up, line protocol is up  
            Internet address is 192.168.1.1/24, Area 0  
            Process ID 56, Router ID 2.2.2.2, Network Type POINT-TO-POINT, Cost: 1  
            Transmit Delay is 1 sec, State POINT-TO-POINT,  
            Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5  
               No Hellos (Passive interface)   #<<<<<< Пассивный интерфейс  
            Index 1/1, flood queue length 0  
            Next 0x0(0)/0x0(0)  
            Last flood scan length is 1, maximum is 1  
            Last flood scan time is 0 msec, maximum is 0 msec  
            Suppress hello for 0 neighbor(s)  
  
   5. Изменение базовой пропускной способности для маршрутизаторов.
  
            R1(config)# router ospf 56  
            R1(config-router)# auto-cost reference-bandwidth 1000  
            R1(config-router)# end  
            R1# clear ip ospf process  
  
            R2(config)# router ospf 56  
            R2(config-router)# auto-cost reference-bandwidth 1000  
            R2(config-router)# end  
            R2# clear ip ospf process  
              
        Сообщение в консоли:  
  
            R1#  
            00:27:30: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Adjacency forced to reset  
  
            00:27:30: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached  
  
            00:28:30: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done  
  
            R2#  
            00:27:39: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Adjacency forced to reset  
  
            00:27:39: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached  
  
            00:28:30: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done  
          
### 4. Убедитесь, что оптимизация OSPFv2 реализовалась.   
  
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
 