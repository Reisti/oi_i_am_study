r2#show running-config   
Building configuration...  
  
Current configuration : 1196 bytes  
!  
version 15.4  
no service timestamps log datetime msec  
no service timestamps debug datetime msec  
no service password-encryption  
!  
hostname r2  
!  
enable secret 5 $1$mERr$9cTjUIEqNGurQiFU.ZeCi1  
!  
ip cef  
ipv6 unicast-routing  
!  
no ipv6 cef  
!  
ipv6 dhcp pool R2-STATEFUL  
 address prefix 2001:db8:acad:3:aaa::/80 lifetime 172800 86400  
 dns-server 2001:DB8:ACAD::254  
 domain-name STATEFUL.com  
!  
username admin secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0  
!  
ip ssh version 2  
no ip domain-lookup  
ip domain-name r2.local  
!  
!  
spanning-tree mode pvst  
!  
interface GigabitEthernet0/0/0  
 no ip address  
 duplex auto  
 speed auto  
 ipv6 address FE80::2 link-local  
 ipv6 address 2001:DB8:ACAD:2::2/64  
!  
interface GigabitEthernet0/0/1  
 no ip address  
 duplex auto  
 speed auto  
 ipv6 address FE80::1 link-local  
 ipv6 address 2001:DB8:ACAD:3::1/64  
 ipv6 nd other-config-flag  
 ipv6 dhcp server R2-STATEFUL  
!  
interface Vlan1  
 no ip address  
 shutdown  
!  
ip classless  
!  
ip flow-export version 9  
!  
ipv6 route ::/0 2001:DB8:ACAD:2::1  
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