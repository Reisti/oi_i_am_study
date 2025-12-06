
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
line con 0  
!  
login  
line vty 5 15  
login  
!  
  
end  
