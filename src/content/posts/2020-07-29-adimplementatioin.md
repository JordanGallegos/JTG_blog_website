---
template: blog-post
title: Implementing Active Directory On-Premise
slug: /active-directory-onprem
date: 2021-07-31 12:00
description: Creating AD Forest
featuredImage: /assets/activedirectory.png
---

#Purpose#
Implement an Active Directory environment containing four servers and two client desktops. The servers will run various roles and features including DNS, DHCP, Print Server and WSUS. This lab will provide me further efficiency relating to AD creation. And provide an introduction into the maintenance and implementation of DNS, DHCP, Print Server and WSUS on Windows Servers. 

###Topology###
![](/screenshots/activedirectoryonprem/topology.png)  

- 4 Servers (Windows Server 2016)
  - DC1 | 10.10.10.2
      - DC
      - DNS
      - DHCP
  - DC2 | 10.10.10.3
      - DC
      - DNS  
  - WSUS | 10.10.10.5 and DHCP (Internet access)
      - WSUS
  - RODC | 10.10.10.4
      - RODC
      - DNS
  
  <br>
- 2 Clients (Windows 10) | ip via dhcp

- Network Switch (gateway) | 10.10.10.1
  - interconnects all devices on vlan 100

##Configuration##
####Switch####
- Configured cisco switch to act as a gateway (10.10.10.1) for the network
- Set all ports to access the same VLAN
```
interface GigabitEthernet0/0
 switchport access vlan 100
 switchport mode access
 negotiation auto
!
interface GigabitEthernet0/1
 switchport access vlan 100
 switchport mode access
 negotiation auto
!
interface GigabitEthernet0/2
 switchport access vlan 100
 switchport mode access
 negotiation auto
!
interface GigabitEthernet0/3
 switchport access vlan 100
 switchport mode access
 negotiation auto
!
interface GigabitEthernet1/0
 switchport access vlan 100
 switchport mode access
 negotiation auto
!
interface GigabitEthernet1/1
 switchport access vlan 100
 switchport mode access
 negotiation auto
!
interface Vlan100
 ip address 10.10.10.1 255.255.255.0
```
####DC1####
####Add roles and features to DC1#####
- Installed Domain Services, DNS, and DHCP roles on DC1  

![](/screenshots/activedirectoryonprem/DC1roles.png)  

- Installation successful, post deployment configuration needed  

![](/screenshots/activedirectoryonprem/dc1post.png)  

####Domain Controller Promotion#####

- Promoted DC1 to Domain Controller  

- Created new forest called: *local.jordantg.com*  

![](/screenshots/activedirectoryonprem/newforest.png)

- Set root domain password  


- Set NetBIOS domain name

![](/screenshots/activedirectoryonprem/netbios.png)  

- Set Paths as default  and install

- Successfully created new domain LOCAL  

![](/screenshots/activedirectoryonprem/DCup.png)  


####DHCP Configuration####

- Kept defaults for DHCP post deployment  

- Created new scope from DHCP tool installed under administrative tools  

![](/screenshots/activedirectoryonprem/newscope.png) 

- Named scope *local.jordantg.com*  

- Set scope to 10.10.10.70 - 10.10.10.150  

![](/screenshots/activedirectoryonprem/setscope.png) 

- No exlusions needed as static IP addresses needed for the lab are set to IPv4 addresses out of the DHCP scope  

- Set lease time to default (8 days)  

- Set router IP address (gateway) as 10.10.10.1  

- Set other DNS server IP addresses to other servers  

![](/screenshots/activedirectoryonprem/dhcpdns.png) 

####DC2####
####Add roles and features to DC2#####  

- Installed Domain Serivces and DNS  

![](/screenshots/activedirectoryonprem/dc2roles.png) 

####Domain Controller Promotion####  

- Set DC2 to connect as a domain controller from an existing domain  

![](/screenshots/activedirectoryonprem/dc2connect.png)

- Used login credentials for admin account on DC1  

![](/screenshots/activedirectoryonprem/dc2creds.png) 

- Used DSRM password set during initial forest creation and DNS server


![](/screenshots/activedirectoryonprem/dsrmpw.png) 

- Unchecked DNS delegation, it will be configured manually from DC1 (parent zone)

- Selected defaults for remaining steps  

- DC2 joined to *local.jordantg.com* domain as a domain controller  

![](/screenshots/activedirectoryonprem/d2joined.png) 


####RODC####  

####Set RODC as a read only domain controller##### 

- The Read Only Domain Controller will provide domain resiliency and a non-writable domain controller to a remote site

- Installed Domain Services and checked DNS  

![](/screenshots/activedirectoryonprem/rodcroles.png) 

####Read Only Domain Controller Promotion####

- Used login credentials for DC1 to connect domain to *local.jordantg.com*  

![](/screenshots/activedirectoryonprem/rodcconnect.png) 

- Used DSRM password set during initial forest creation, checked DNS and RODC

![](/screenshots/activedirectoryonprem/rodcdsrm.png) 

- Delegated the administrator account on DC1 as the account allowed to replicate passwords to RODC  

![](/screenshots/activedirectoryonprem/rodcconfigure.png) 

- Unchecked DNS delegation, it will be configured manually from DC1 (parent zone)

- Selected defaults for remaining steps  

- RODC joined to *local.jordantg.com* domain as a domain controller  

![](/screenshots/activedirectoryonprem/rodcjoined.png) 

####WSUS####  

- Connected WSUS to *local.jordantg.com* domain  

![](/screenshots/activedirectoryonprem/wsusjoined.png) 

####Add roles and features to WSUS#####  

- Installed WSUS Services and all dependencies  

![](/screenshots/activedirectoryonprem/wsusroles.png)

- Used WID Connectivity for updates  

![](/screenshots/activedirectoryonprem/wid.png) 

- Created folder on C: drive to store updates  

![](/screenshots/activedirectoryonprem/updates.png) 

- Set defaults for IIS instance  

- Launched Windows Server Update Services  

![](/screenshots/activedirectoryonprem/wsusconfig.png) 

- Set defaults for Upstream Server and Proxy Server  

- Tested connection to Microsoft Upstream Server  

- Only downloaded English updates  

- Selected updates for Windows 10 clients only  

![](/screenshots/activedirectoryonprem/win10updates.png) 

- Selected Critical, Definition, and Security updates  

![](/screenshots/activedirectoryonprem/criticalonly.png) 

####DC1#### 

- Created new group policy object from domain controller for WSUS  

![](/screenshots/activedirectoryonprem/newgpo.png) 

- Edited Group Policy Object  

![Configure Automatic Updates](/screenshots/activedirectoryonprem/gpo1.png) 

![Configured target group: clients](/screenshots/activedirectoryonprem/gpo2.png) 

![Specified location of WSUS server](/screenshots/activedirectoryonprem/gpo3.png) 

- Added WSUS Clients to WSUS Update Services  

![](/screenshots/activedirectoryonprem/wsusclients.png) 

#Conclusion#  

- Confirmed DNS working (Forward Lookup Zones for all six endpoints)  

![](/screenshots/activedirectoryonprem/dnscheck.png) 

- Confirmed DHCP working (Two clients computers obtained leases)  

![](/screenshots/activedirectoryonprem/dhcpworking.png) 

- Confirmed all Domain Controllers promoted successfully (DC1, DC2, and RODC)  

![](/screenshots/activedirectoryonprem/dcgood.png) 

- Confirmed client is discoverable on WSUS server in Update Services  

![](/screenshots/activedirectoryonprem/clientreachable.png) 

- Confirmed client is receiving updates from organization  

![](/screenshots/activedirectoryonprem/clientupdate.png) 

###Final Thoughts###  

This lab served as a catch-all to gain experience with several roles and features in Windows Server. I had previous exposure to active directory forest creation. But DNS, DHCP, and WSUS implementation were entirely new concepts introduced to me in this lab. The DNS and DHCP features were added without any issues. I was able to verify proper DNS forward lookups from the administrative DNS tool installed on DC1. The DHCP administrative tool on DC1 allowed me to verify proper DHCP leases. The most difficult portion of this lab was the WSUS implementation. WSUS was an entirely new concept to me. While I had not implemented some of the other roles in the past, I still had a comfortable knowledge of the theory. For WSUS, I did not have any previous theoretical or practical knowledge. The issue that arose while implementing WSUS was related to the clients not populating in the Update Services table on the WSUS server. After manually trying to point the clients to the update server (10.10.10.5:8530), I resolved the issue by defining the update server in the default domain policy of *local.jordantg.com*. 

I improved my overall competency with Windows Server as this lab was rich with challenges. If I were to add to this lab in the future, I would add another server and configure it to be a print server and file server. It would allow me to emulate a client connecting to a network share and network printer.  
