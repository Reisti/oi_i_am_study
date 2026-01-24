S1#show running-config   
Building configuration...  
  
Current configuration : 1311 bytes  
!  
version 15.0  
no service timestamps log datetime msec  
no service timestamps debug datetime msec  
no service password-encryption  
!  
hostname S1  
!  
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1  
!  
!  
!  
ip ssh version 2  
no ip domain-lookup  
ip domain-name s1.local  
!  
username admin secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0  
!  
!  
spanning-tree mode pvst  
spanning-tree extend system-id  
!  
interface FastEthernet0/1  
!  
interface FastEthernet0/2  
!  
interface FastEthernet0/3  
!  
interface FastEthernet0/4  
!  
interface FastEthernet0/5  
!  
interface FastEthernet0/6  
!  
...  
!  
interface FastEthernet0/24  
!  
interface GigabitEthernet0/1  
!  
interface GigabitEthernet0/2  
!  
interface Vlan1  
 ip address 192.168.1.11 255.255.255.0  
!  
ip default-gateway 192.168.1.1  
!  
banner motd ^C  
!!!!!!!!!!!!!! Under the protection of the Adeptus Mechanicus !!!!!!!!!!!!!!  
^C  
!  
line con 0  
 login local  
!
line vty 0 4  
 login local  
 transport input ssh  
line vty 5 15  
 login local  
!  
end  