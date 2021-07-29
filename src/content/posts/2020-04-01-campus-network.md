---
template: blog-post
title: Two-Tier Campus Network
slug: /campus-network
date: 2021-04-01 12:47
description: Two-Tier Campus Network
featuredImage: /assets/campuslab.png
---

#Purpose#
To design and demonstrate an operable two-tier collapsed network. My topology would replicate a network of a small to medium-sized enterprise.

###Topology###
- Edge router used for routing traffic to ISP
  - Gi0/2 statically assigning IP address from ISP (lab network) of 192.168.13.105
  - Gi0/0-1, both configured for inside NAT
  - Access-list permitting traffic from Gi0/2 to 10.0.0.0/16 network
  - In a three-tier design, instead of a router, additional switches can be placed above the distribution multilayer switches. These L3 switches would be referred to as the Core switches
- Two distribution switches
  - Dynamic routing protocol (EIGRP)
  - HSRP between Gi0/0
  - VLANs 13, 100
    - SVIs
      - VLAN 13 | 10.0.13.254/24
      - VLAN 100 | 10.0.100.254/24
- Four access switches
    - VLANs 13, 100

###Configuration###
####Access Switches####
####A1, A2, A3####
![](/screenshots/campuslab/a1a2a3.png)
- Configured to allow vlan 100 along access and trunk ports  
- Gi0/0-0/1 configured for trunking  
- Gi0/2 configured for endpoints  

####A4####
![](/screenshots/campuslab/a4.png)  
- Configured to allow vlan 13 along access and trunk ports
- Gi0/0-0/1 configured for trunking
- Gi0/2 configured for endpoints  

####Distribution Switches####
####D1####
![](/screenshots/campuslab/d1.png) 
- Gi0/2-3 and Gi1/0-1 configured for trunking
- Gi0/0-1 enabled for IP routing
- Gi0/0 used as HSRP link between D1 & D2  

![](/screenshots/campuslab/hsrp.png) 
- Int VLAN 13 created
  - Set to ip address 10.0.13.254 to allow VLAN 13 traffic from access layer
  - Virtual ip address assigned as 10.0.13.1 for HSRP (same as D2)
- Int VLAN 100 created
  - Set to ip address 10.0.100.254 to allow VLAN 100 traffic from access layer
  - Virtual ip address assigned as 10.0.100.1 for HSRP (same as D2)  

![](/screenshots/campuslab/eigrp.png) 
- EIGRP enabled as dynamic routing protocol for 10.0.0.0/16 network
####D2####
![](/screenshots/campuslab/d2.png) 
- Gi0/2-3 and Gi1/0-1 configured for trunking
- Gi0/0-1 enabled for ip routing
- Gi0/0 used as HSRP link between D1 & D2  

![](/screenshots/campuslab/d2vlan.png) 
- Int VLAN 13 created
    - Set to ip address 10.0.13.253 to allow VLAN 13 traffic from access layer
    - Virtual ip address assigned as 10.0.13.1 for HSRP (same as D1)
- Int VLAN 100 created
    - Set to ip address 10.0.100.253 to allow VLAN 100 traffic from access layer
    - Virtual ip address assigned as 10.0.100.1 for HSRP (same as D1)  

![](/screenshots/campuslab/d2eigrp.png) 
- EIGRP enabled as dynamic routing protocol for 10.0.0.0/16 network  

![](/screenshots/campuslab/d2route.png) 

####Router####
####Edge Router####
![](/screenshots/campuslab/edgerouter.png) 
- Gi0/0
  - Physical link to switch D1
  - Set to NAT inside from ISP IP address
- Gi0/1
  - Physical link to switch D2
  - Set to NAT inside from ISP IP Address
- Gi0/2
  - Physical link externally connected to ISP
  - Set to NAT outside from internal LAN network 10.0.0.0/16  

  ![](/screenshots/campuslab/edgerouternat.png) 
  - Access-list permitting traffic from 10.0.0.0/16 network
  - Set to NAT inside source address from the 10.0.0.0/16 network translated as ISP-given ip address 192.168.1.15 (Gi0/2)  

  ![](/screenshots/campuslab/edgeroutereigrp.png) 
  - EIGRP enabled as dynamic routing protocol for 10.0.0.0/16 network  

  ![](/screenshots/campuslab/edgerouterroutes.png) 

#Conclusion#
![](/screenshots/campuslab/conclusion.png) 
- Confirmed I was able to reach eve-ng instance at 192.168.13.103 from 10.0.100.5 (VLAN 100,user)  

![](/screenshots/campuslab/conclusion2.png) 
- Confirmed I was able to reach eve-ng instance at 192.168.13.103 from 10.0.13.5 (VLAN 13, mgmt)

###Final Thoughts###  
This lab was a good refresher and will be one that I continue to circle back to keep this network design fresh in my mind. HSRP is a protocol that was familiar to me from studying, but this is the first time I have implemented it in a lab. Next time when designing, I will keep in my mind to keep the first ip address for each VLAN unconfigured. I ran into an issue and had to re-scheme my ip addressing because HSRP uses the first ip address in the VLAN's subnet.
- If I were to continue to improve this lab, I would implement the following:
    - Add additional access-lists to only allow ssh access into switches from clients in VLAN 13 (mgmt)
    - Add a route into a "Datacenter", within the datacenter create DHCP and authentication servers
    - DHCP to not have to manually set client ip addresses
    - Authentication server to tie ssh access using AAA
