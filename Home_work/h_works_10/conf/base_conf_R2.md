R2#show running-config   
Building configuration...  
  
Current configuration : 1127 bytes  
!  
version 15.4  
no service timestamps log datetime msec  
no service timestamps debug datetime msec  
no service password-encryption  
!  
hostname R2  
!  
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1  
!  
no ip cef  
no ipv6 cef  
!  
username admin secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0  
!  
ip ssh version 2  
no ip domain-lookup  
ip domain-name R2.local  
!  
spanning-tree mode pvst  
!  
interface Loopback1  
 ip address 192.168.1.1 255.255.255.0  
 ip ospf network point-to-point   
 ip ospf priority 1  
!  
interface GigabitEthernet0/0/0  
 no ip address  
 duplex auto  
 speed auto  
 shutdown  
!  
interface GigabitEthernet0/0/1  
 ip address 10.53.0.2 255.255.255.0  
 ip ospf hello-interval 30  
 ip ospf dead-interval 120  
 duplex auto  
 speed auto  
!  
interface Vlan1  
 no ip address  
 shutdown  
!  
router ospf 56  
 router-id 2.2.2.2  
 log-adjacency-changes  
 network 10.53.0.0 0.0.0.255 area 0  
 network 192.168.1.0 0.0.0.255 area 0  
!  
ip classless  
!  
ip flow-export version 9  
!  
banner motd ^C Adeptus Mechanicus ^C  
!  
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