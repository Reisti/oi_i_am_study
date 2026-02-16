s2#show running-config   
Building configuration...  
  
Current configuration : 1612 bytes  
!  
version 15.0  
no service timestamps log datetime msec  
no service timestamps debug datetime msec  
no service password-encryption  
!  
hostname s2  
!  
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1  
!  
ip ssh version 2  
no ip domain-lookup  
ip domain-name s2.local  
!  
username admin secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0  
!  
spanning-tree mode pvst  
spanning-tree extend system-id  
!  
interface FastEthernet0/1  
 shutdown  
!  
interface FastEthernet0/2  
 shutdown  
!  
interface FastEthernet0/3  
 shutdown  
!  
interface FastEthernet0/4  
 shutdown  
!  
interface FastEthernet0/5  
!  
interface FastEthernet0/6  
 shutdown  
!  
interface FastEthernet0/7  
 shutdown  
!  
interface FastEthernet0/8  
 shutdown  
!  
interface FastEthernet0/9  
 shutdown  
!  
interface FastEthernet0/10  
 shutdown  
!  
interface FastEthernet0/11  
 shutdown  
!  
interface FastEthernet0/12  
 shutdown  
!  
interface FastEthernet0/13  
 shutdown  
!  
interface FastEthernet0/14  
 shutdown  
!  
interface FastEthernet0/15  
 shutdown  
!  
interface FastEthernet0/16  
 shutdown  
!  
interface FastEthernet0/17  
 shutdown  
!  
interface FastEthernet0/18  
!  
interface FastEthernet0/19  
 shutdown  
!  
interface FastEthernet0/20  
 shutdown  
!  
interface FastEthernet0/21  
 shutdown  
!  
interface FastEthernet0/22  
 shutdown  
!  
interface FastEthernet0/23  
 shutdown  
!  
interface FastEthernet0/24  
 shutdown  
!  
interface GigabitEthernet0/1  
 shutdown  
!  
interface GigabitEthernet0/2  
 shutdown  
!  
interface Vlan1  
 ip address 192.168.1.98 255.255.255.240  
!  
ip default-gateway 192.168.1.97  
!  
banner motd ^CAdeptus Mechanicus^C  
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
