R1#show running-config   
Building configuration...  
  
Current configuration : 1359 bytes  
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
ip dhcp relay information trust-all  
!  
ip dhcp excluded-address 192.168.10.1 192.168.10.9  
ip dhcp excluded-address 192.168.10.201 192.168.10.202  
!  
ip dhcp pool Student  
 network 192.168.10.0 255.255.255.0  
 default-router 192.168.10.1  
 domain-name CCNA2.Lab-11.6.1  
!  
ip cef  
no ipv6 cef  
!  
username admin secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0  
!  
no ip domain-lookup  
ip domain-name R1.local  
!  
spanning-tree mode pvst  
!  
interface Loopback0  
 ip address 10.10.1.1 255.255.255.0  
!  
interface GigabitEthernet0/0/0  
 no ip address  
 duplex auto  
 speed auto  
 shutdown  
!  
interface GigabitEthernet0/0/1  
 no ip address  
 duplex auto  
 speed auto  
!  
interface GigabitEthernet0/0/1.10  
 description Managment  
 encapsulation dot1Q 10  
 ip address 192.168.10.1 255.255.255.0  
!  
interface GigabitEthernet0/0/1.333  
 encapsulation dot1Q 333 native  
 no ip address  
!  
interface Vlan1  
 no ip address  
 shutdown  
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
line vty 5 15  
 login local  
 transport input ssh  
!  
end  