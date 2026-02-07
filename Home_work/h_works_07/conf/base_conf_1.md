S2#show running-config   
Building configuration...  
  
Current configuration : 1357 bytes  
!  
version 15.0  
no service timestamps log datetime msec  
no service timestamps debug datetime msec  
no service password-encryption  
!  
hostname S2  
!
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1  
!  
ip ssh version 2  
no ip domain-lookup  
ip domain-name S2.local  
!  
username admin secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0  
!  
spanning-tree mode pvst  
spanning-tree extend system-id  
!  
interface FastEthernet0/1  
!  
.....  
!  
interface FastEthernet0/24  
!  
interface GigabitEthernet0/1  
!  
interface GigabitEthernet0/2  
!  
interface Vlan1  
ip address 192.168.1.3 255.255.255.0  
!  
banner motd ^CAdeptus Mechanicus ^C  
!  
line con 0  
logging synchronous  
login local  
!  
line vty 0 4  
login local  
transport input ssh  
line vty 5 15  
login local  
!  
end  