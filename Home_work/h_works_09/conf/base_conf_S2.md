S2#show running-config   
Building configuration...  
  
Current configuration : 3046 bytes  
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
no ip domain-lookup  
ip domain-name s2.local  
!  
username admin secret 5 $1$mERr$hx5rVt7rPNoS4wqbXKX7m0  
!  
spanning-tree mode pvst  
spanning-tree extend system-id  
!  
interface FastEthernet0/1  
 switchport trunk native vlan 333  
 switchport trunk allowed vlan 10  
 switchport mode trunk 
 switchport nonegotiate   
!  
interface FastEthernet0/2  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/3  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/4  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/5  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/6  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/7  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/8  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/9  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!   
interface FastEthernet0/10  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/11  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/12  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/13  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/14  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/15  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/16  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/17  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/18  
 switchport access vlan 10  
 switchport mode access  
!  
interface FastEthernet0/19  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!   
interface FastEthernet0/20  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/21  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/22  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/23  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface FastEthernet0/24  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface GigabitEthernet0/1  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface GigabitEthernet0/2  
 switchport access vlan 999  
 switchport mode access  
 shutdown  
!  
interface Vlan1  
 no ip address  
 shutdown  
!  
interface Vlan10  
 ip address 192.168.10.202 255.255.255.0  
!  
banner motd ^C Adeptus Mechanicus ^C  
!  
line con 0  
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