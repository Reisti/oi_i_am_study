sw-homework-01# show running-config  
Building configuration...  
  
Current configuration : 1299 bytes  
!  
version 15.0  
no service timestamps log datetime msec  
no service timestamps debug datetime msec  
service password-encryption  
!  
hostname sw-homework-01  
!  
enable password 7 0824424F0B1500141B180F0B  
!  
no ip domain-lookup  
ip host 192.168.1.2 255.255.255.0   
!  
username admin privilege 1 password 7 0822455D0A16  
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
ip address 192.168.1.2 255.255.255.0  
!  
banner motd ^C  
Hello Word  
^C  
!  
line con 0  
logging synchronous  
!  
line vty 0 4  
login  
line vty 5 15  
login  
!  
end  
