R1#show running-config   
Building configuration...  
  
Current configuration : 935 bytes  
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
ipv6 unicast-routing  
!  
no ipv6 cef  
!  
username admin secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0  
!  
ip ssh version 2  
ip domain-name r1.local  
!  
!  
spanning-tree mode pvst  
!  
interface GigabitEthernet0/0/0  
 no ip address  
 duplex auto  
 speed auto  
 ipv6 address FE80::1 link-local  
 ipv6 address 2001:DB8:ACAD:A::1/64  
!  
interface GigabitEthernet0/0/1  
 no ip address  
 duplex auto  
 speed auto  
 ipv6 address FE80::1 link-local  
 ipv6 address 2001:DB8:ACAD:1::1/64  
!  
interface Vlan1  
 no ip address  
 shutdown  
!  
ip classless  
!  
ip flow-export version 9  
!  
no cdp run  
!  
line con 0  
!  
line aux 0  
!  
line vty 0 4  
 login local  
 transport input ssh  
line vty 5 15  
 login local  
 transport input ssh  
!  
end  