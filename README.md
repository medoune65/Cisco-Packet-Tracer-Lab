# CISCO PACKET TRACER NETWORKING LAB
In this project i will be using cisco packet tracer to simulate a physical network and apply the networking concepts i have learned.

The networking concepts that I will apply in this lab include:
 - Router Configuration
 - DHCP
 - NAT (Network Address Translation)
 - Routing Protocols (OSPF, BGP, Static Routing, EIGRP and default routes)
 - ACL's (Access Control Lists)


# Designing The Topology
![Alt text](https://github.com/user-attachments/assets/1703f6e5-9853-494c-ab78-2f83d094ccb6)

As you can see from the image above, here is the layout of the topology I will be configuring.
It consists of 11 routers, 7 switches, 7 PC's and 2 web servers, i will be configuring everything on the network to be able to communicate with each other. 

# Steps to get the network up and running:
 1. Configure IP addresses on all router interfaces
 2. Configure stati IP addresses on PC's A, B, and C as well as web servers 1 and 2
 3. Configure routers HFC-Home and DSL-Home to serve as DHCP servers for PC's D, E and F and G
 4. Configure PC's D, E, F and G to get a DHCP address from their gateway routers (HFC-Home will serve as a DHCP server for PC's D and E, DSL-Home 
    will serve as a DHCP server for PC's F and G)
 5. Since PC's D,E,F and G have private IP addresses configure Dynamic NAT on routers HFC-Home and DSL-Home so they can communicate with the outside networks
 6. Since Web servers 1 and 2 have private IP addresses we will be configuring static NAT on router 7, so that each web server will be assigned its own public IP address when communicating with the outside networks
 7. Configure routing protocols:
    - OSPF on network AS2500
    - Static routing on network AS2700
    - EIGRP on network AS2100
    - OSPF on network AS2200
8. Configure BGP between border routers R1, R2, R5 and R6 as well as redistribution to translate the interior gateway protocols into BGP
9. Configure an ACL to ban anyone not belonging in the HFC network from accessing the HFC-Network router
   via Telnet
10. Configure an ACL to ban computers in the 33.0.0.0/24 network from browsing the web on web servers 1 and 2, but allow all other
communication


# Configuring The Network
1. Logged into the command terminal of all routers and configured IP addresses on all router interfaces

          R1(config)#interface GigabitEthernet 0/1
   
          R1(config-if)#ip address 13.0.81.1 255.255.255.0
   
          R1(config-if)#no shutdown


   2. Went into the IP configuration settings of PC's A, B, C and web servers 1 and 2 and configured a static ip address, subnet mask and default 
      gateway

      ![Alt text](https://github.com/user-attachments/assets/6cbac32e-2da7-4a5b-8445-1cb205fcd638)


   3. Configured routers HFC-Home and DSL-Home to serve as DHCP ervers for PC's D, E, F and G and set PC's D, E, F and G to receive a DHCP IP 
      address
  
           HFC-H(config)#ip dhcp pool HFCHomePool

           HFC-H(dhcp-config)#network 192.168.1.0 255.255.255.0

           HFC-H(dhcp-config)#default-router 192.168.1.1

           “DSL-Home” router configuration is the same.


   4. Configured dynamic NAT on routers HFC-Home and DSL-Home to translate the internal private IP's of PC's D, E, F and G into public IP's

             First we configure an access list with a range of IP's:

                    HFC-H(config)#ip access-list standard 1
              
                    HFC-H(config-std-nacl)#permit 192.168.1.0 0.0.0.255

             Than we use that access list to apply NAT to the appropriate interface:

                    HFC-H(config)#ip nat inside source list 1 interface GigabitEthernet 0/0 overload
              
                    HFC-H(config)#interface GigabitEthernet 0/0
              
                    HFC-H(config-if)#ip nat outside
              
                    HFC-H(config)#interface GigabitEthernet 0/1
              
                    HFC-H(config-if)#ip nat inside

      5. Configured static NAT on router 7 so that web servers 1 and 2 will each get their own public IP addresses to communicate with the outside 
         networks
     
                    R7(config)#ip nat inside source static 10.0.1.1 20.0.0.3

                    R7(config)#ip nat inside source static 10.0.1.2 20.0.0.4
                    
                    R7(config)#ip nat inside source static 10.0.2.1 20.0.0.5
                    
                    R7(config)#ip nat inside source static 10.0.2.2 20.0.0.6
                    
                    R7(config)#interface GigabitEthernet 0/0
                    
                    R7(config-if)#ip nat inside
                    
                    R7(config)#interface GigabitEthernet 0/2
                    
                    R7(config-if)#ip nat inside
                    
                    R7(config)#interface GigabitEthernet 0/1
                    
                    R7(config-if)#ip nat outside 


    6. Configured routing protocols on routers on all networks:
          - OSPF on network AS2500

               R3(config)#router ospf 1

               R3(config-router)#network 31.0.0.0 0.0.0.7 area 0

               R3(config-router)#network 32.0.0.0 0.0.0.255 area 0
         
          - Static routing on network AS2700

              R7(config)#ip route 0.0.0.0 0.0.0.0 GigabitEthernet 0/1
         
          - EIGRP on network AS2100

              R1(config)#router eigrp 2100

               R1(config-router)#no auto-summary
   
               R1(config-router)#network 13.0.81.0 0.0.0.255
         
          - OSPF on network AS2200

                DSL-H(config)#router ospf 1

                DSL-H(config-router)#network 99.0.248.0 0.0.0.255 area 0

                DSL-H(config-router)#network 192.168.1.0 0.0.0.255 area 0


      7. Configured BGP between border routers R1, R2, R5 and R6 as well as redistribution to translate the interior gateway protocols into BGP

                 R2(config)#router bgp 2500

                 R2(config-router)#network 5.1.0.0 mask 255.255.255.252
                 
                 R2(config-router)#neighbor 5.1.0.1 remote-as 2100

                 R2(config)#router bgp 2500
                 
                 R2(config-router)#redistribute ospf 1
      
      
      8.  Configured a standard ACL to ban anyone not belonging in the HFC network from accessing the HFC-Network router
          via Telnet


                 HFC-N(config)#ip access-list standard 1

                 HFC-N(config-std-nacl)#permit 13.0.81.0 0.0.0.255
                 
                 HFC-N(config-std-nacl)#permit 183.2.0.0 0.0.0.255
                 
                 HFC-N(config)#line vty 0 15
                 
                 HFC-N(config-line)#access
                 
                 HFC-N(config-line)#access-class 1 in


      9. Configured an ACL to ban computers in the 33.0.0.0/24 network from browsing the web on web servers 1 and 2, but allowed all other
         communication

                 R4(config)#ip access-list extended 101

                 R4(config-ext-nacl)#deny tcp 33.0.0.0 0.0.0.255 any eq www
                 
                 R4(config-ext-nacl)#permit ip any any
                 
                 R4(config)#interface GigabitEthernet 0/2
                 
                 R4(config-if)#ip access-group 101 in

       10. Now that everything is configured we can ping devices to test if they're successful
