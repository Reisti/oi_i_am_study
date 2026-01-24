R1#show running-config   
Building configuration...  
  
Current configuration : 756 bytes  
!  
version 15.4  
no service timestamps log datetime msec  
no service timestamps debug datetime msec  
no service password-encryption  
!  
hostname R1  
!  
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1  
!  
ip cef  
no ipv6 cef  
!  
!  
!  
username admin privilege 15 secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0  
!  
!  
!  
!  
!  
!  
!  
!  
ip ssh version 2  
no ip domain-lookup  
ip domain-name s1.local  
!  
!  
spanning-tree mode pvst  
!  
interface GigabitEthernet0/0/0  
 no ip address  
 duplex auto  
 speed auto  
 shutdown  
!
interface GigabitEthernet0/0/1  
 ip address 192.168.1.1 255.255.255.0  
 duplex auto  
 speed auto  
!  
interface Vlan1  
 no ip address  
 shutdown  
!  
ip classless  
!  
ip flow-export version 9  
!  
banner motd ^C  
!!!!!!!!!!!!!! Under the protection of the Adeptus Mechanicus !!!!!!!!!!!!!!  
^C   
line con 0  
 login local  
!  
line aux 0  
 login local  
!  
line vty 0 4  
 login local  
 transport input ssh  
!  
end  