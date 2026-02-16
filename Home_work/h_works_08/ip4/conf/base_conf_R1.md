R1#show running-config   
Building configuration...  
  
Current configuration : 1528 bytes  
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
!  
ip dhcp excluded-address 192.168.1.1 192.168.1.5  
ip dhcp excluded-address 192.168.1.97 192.168.1.101  
!  
ip dhcp pool lan100  
 network 192.168.1.0 255.255.255.192  
 default-router 192.168.1.1  
 domain-name CCNA-lab.com  
ip dhcp pool client_lan  
 network 192.168.1.96 255.255.255.240  
 default-router 192.168.1.97  
 domain-name CCNA-lab.com  
!  
ip cef  
no ipv6 cef  
!  
username admin secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0  
!  
ip ssh version 2  
no ip domain-lookup  
ip domain-name r1.local  
!  
!  
spanning-tree mode pvst  
!  
interface GigabitEthernet0/0/0  
 ip address 10.0.0.1 255.255.255.252  
 duplex auto  
 speed auto  
!  
interface GigabitEthernet0/0/1  
 no ip address  
 duplex auto  
 speed auto  
!  
interface GigabitEthernet0/0/1.100  
 description clien_vlan  
 encapsulation dot1Q 100  
 ip address 192.168.1.1 255.255.255.192  
!  
interface GigabitEthernet0/0/1.200  
 description Managment_admin_vlan  
 encapsulation dot1Q 200  
 ip address 192.168.1.65 255.255.255.224  
!  
interface GigabitEthernet0/0/1.1000  
 encapsulation dot1Q 1000 native  
 no ip address  
!  
interface Vlan1  
 no ip address  
 shutdown  
!  
ip classless  
ip route 0.0.0.0 0.0.0.0 10.0.0.2   
!  
ip flow-export version 9  
!  
banner motd ^C Adeptus Mechanicus^C  
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