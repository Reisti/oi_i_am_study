#  Лабораторная работа - Настройка протоколов CDP, LLDP и NTP 

###  Задание:  

 1. Создание сети и настройка основных параметров устройства.   
 2. Обнаружение сетевых ресурсов с помощью протокола CDP.    
 3. Обнаружение сетевых ресурсов с помощью протокола LLDP.      
 4. Настройка и проверка NTP.    
 
###  Дано:
#### Таблица адресации:
| Устройство   | Интерфейс   | IP-адрес        | Маска подсети   | Шлюз по умолчанию |  
|-------------:|:------------|:----------------|:----------------|:------------------|  
| R1           | Loopback1   | 172.16.1.1      | 255.255.255.0   |                   |     
|              | G0/0/1      | 10.22.0.1       | 255.255.255.0   |                   |   
| S1           | VLAN 1      | 10.22.0.2       | 255.255.255.0   | 10.22.0.1         |  
| S2           | VLAN 1      | 10.22.0.3       | 255.255.255.0   | 10.22.0.1         |   

#### Топология:  
   ![Топология](image.png)  
  
###  Решение:  
 
###  1.Создание сети и настройка основных параметров устройства;   
  1. Создаем сеть согласно топологии.  
          
  2. Настрайваем параметры для устройств  

      [Получим результат S1;][def]  
      
      [Получим результат S2;][def1]  

      [Получим результат R1;][def2]  

###  2. Обнаружение сетевых ресурсов с помощью протокола CDP.      

   1. На R1 используем соответствующую команду show cdp, чтобы определить, сколько интерфейсов включено CDP, сколько из них включено и сколько отключено.   
    
            R1#show cdp interface  
            Vlan1 is administratively down, line protocol is down  
            Sending CDP packets every 60 seconds  
            Holdtime is 180 seconds  
            GigabitEthernet0/0/0 is administratively down, line protocol is down  
            Sending CDP packets every 60 seconds  
            Holdtime is 180 seconds  
            GigabitEthernet0/0/1 is up, line protocol is up  
            Sending CDP packets every 60 seconds  
            Holdtime is 180 seconds  
  
            Интерфейсов с включённым CDP: 3  
            Активных: 1   

   2. На R1 используем соответствующую команду show cdp, чтобы определить версию IOS, используемую на S1.   
  
            R1#show cdp entry S1  
  
            Device ID: S1  
            Entry address(es):   
            IP address : 10.22.0.2  
            Platform: cisco 2960, Capabilities: Switch  
            Interface: GigabitEthernet0/0/1, Port ID (outgoing port): FastEthernet0/5  
            Holdtime: 168  
   
            Version :  
            Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)  
            Technical Support: http://www.cisco.com/techsupport  
            Copyright (c) 1986-2013 by Cisco Systems, Inc.  
            Compiled Wed 26-Jun-13 02:49 by mnguyen  
  
            advertisement version: 2  
            Duplex: full  
              
            Какая версия IOS используется на  S1?  
                Version 15.2(4)  

   3. На S1 используем соответствующую команду show cdp, чтобы определить, сколько пакетов CDP было выданных.   
  
    Так как show cdp traffic отсутствует в Cisco PT  
    Используем команду  "show cdp neighbors ", чтоб подтвердить активный обмен пакетами.   

            S1#show cdp neighbors   
            Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge  
                            S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone  
            Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID  
            R1           Fas 0/5          153            R       ISR4300     Gig 0/0/1  
            S2           Fas 0/1          153            S       2960        Fas 0/1  
  
   4. Отключить CDP глобально на всех устройствах .  

            R1(config)#no cdp run 
            S1(config)#no cdp run
            S2(config)#no cdp run
            
            Проверим:
            R1#show cdp 
            % CDP is not enabled
        
            Добавился IP адресс
 
  ###  3. Обнаружение сетевых ресурсов с помощью протокола LLDP.   

   1. На R1 очистим текущие трансляции и статистику.   
  
            R1#clear ip nat translation *
            R1#clear ip nat statistics (Нету в PT) 

   2. На R1 настроим команду NAT, необходимую для статического сопоставления внутреннего адреса с внешним адресом.  
  
            ip nat inside source static 192.168.1.2 209.165.200.229   

   3. Протестируем и проверим конфигурацию.   
    
   a.	Давайте проверим, что статический NAT работает. На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations, и вы увидите статическое сопоставление.    
          
            R1#show ip nat translations   
            Pro  Inside global     Inside local       Outside local      Outside global  
            ---  209.165.200.229   192.168.1.2        ---                ---  
    
   b.	Таблица перевода показывает, что статическое преобразование действует. Проверьте это, запустив ping  с R2 на 209.165.200.229. Плинги должны работать.  
           
            R2#ping 209.165.200.229  
            Type escape sequence to abort.  
            Sending 5, 100-byte ICMP Echos to 209.165.200.229, timeout is 2 seconds:  
            !!!!!  
            Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms  
     
   c.	На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations, и вы увидите статическое сопоставление и преобразование на уровне порта для входящих pings.  
          
            R1#show ip nat translations   
            Pro  Inside global     Inside local       Outside local      Outside global  
            icmp 209.165.200.229:45192.168.1.2:45     209.165.200.225:45 209.165.200.225:45  
            ---  209.165.200.229   192.168.1.2        ---                ---  

          

[def]: conf/base_conf_S1.md   
[def1]: conf/base_conf_S2.md    
[def2]: conf/base_conf_R1.md   
  
 