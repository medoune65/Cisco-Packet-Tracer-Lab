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







