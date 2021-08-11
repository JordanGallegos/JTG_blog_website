---
template: blog-post
title: Azure AD Connect to On-Prem
slug: /azureadconnect-to-onprem
date: 2021-08-01 12:00
description: Azure AD Connect to On-Prem
featuredImage: /assets/AzureADConnect.jpeg
---

#Purpose#  

To successfully implement an Active Directory Domain on Windows Server 2019 (on-prem) and connect to Azure AD (cloud). I'll be using Azure AD connect to sync the two sources together. Once they are properly syncing I will create a user on-prem and verify the user populated in the Azure portal. Once properly synced, the user will be assigned an O365 license. Afterwards I will log into a Windows 10 client connected to the domain using the new user account and verify SSO works when attempting to log into O365. 

###Topology###  

- Windows Server 2019  (on-prem)
  - Domain Controller 
- Azure Active Directory 
  - jordantg.com
  - Azure AD Connect  
	- O365  
<br>
- Windows 10 Client to join domain  

##Configuration##  

####On-Prem Domain Controller Promotion####  

- Selected Active Directory Domain Services server role  

![](/screenshots/adconnect/adcreation.png) 

- Left defaults for features 


- Started post deployment installation
- Specified Root Domain Name  

![](/screenshots/adconnect/domaincreate.png) 

- Set DSRM password  

![](/screenshots/adconnect/dsrmpw.png) 

- Set NetBIOS domain name  

![](/screenshots/adconnect/netbios.png) 

- Left defaults for remaining steps and installed domain controller

![](/screenshots/adconnect/prereq.png) 

- Connected to jordantg.com domain  

![](/screenshots/adconnect/jtgdomain.png) 
 
####Connecting to Azure AD Connect####  

- Logged into Azure portal for *jordantg.com*  

![](/screenshots/adconnect/portal.png) 

- Downloaded and installed Azure AD Connect.msi  

- Enabled TLS 1.2 as it is a prerequisite to Azure AD connect  
- Used the following script to enable TLS 1.2
```
New-Item 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -Force | Out-Null

	New-ItemProperty -path 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -name 'SystemDefaultTlsVersions' -value '1' -PropertyType 'DWord' -Force | Out-Null

	New-ItemProperty -path 'HKLM:\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null

	New-Item 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -Force | Out-Null

	New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SystemDefaultTlsVersions' -value '1' -PropertyType 'DWord' -Force | Out-Null

	New-ItemProperty -path 'HKLM:\SOFTWARE\Microsoft\.NETFramework\v4.0.30319' -name 'SchUseStrongCrypto' -value '1' -PropertyType 'DWord' -Force | Out-Null

	New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -Force | Out-Null
	
	New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'Enabled' -value '1' -PropertyType 'DWord' -Force | Out-Null
	
	New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server' -name 'DisabledByDefault' -value 0 -PropertyType 'DWord' -Force | Out-Null
	
	New-Item 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -Force | Out-Null
	
	New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'Enabled' -value '1' -PropertyType 'DWord' -Force | Out-Null
	
	New-ItemProperty -path 'HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client' -name 'DisabledByDefault' -value 0 -PropertyType 'DWord' -Force | Out-Null
	Write-Host 'TLS 1.2 has been enabled.'
```  

- Installed Azure AD required components (left default)  

![](/screenshots/adconnect/adconnectcomponents.png) 

- Selected Password Hash Synchronization as sign-in method  

![](/screenshots/adconnect/hash.png) 

- Logged into Azure AD Connect to connect to Portal

![](/screenshots/adconnect/login.png) 

- Connected *jordantg.com* forest 
- Used Administrator account (Enterprise Admin) to configure directory  

![](/screenshots/adconnect/jordantgforest.png) 

- Selected **Test** Organizational Unit to sync  
  - Users will be added to this OU to verify syncing is working

![](/screenshots/adconnect/oufiltering.png) 

- Left defaults for Identifying user, Filtering, and Optional Features  

- Used Administrator credentials to set SSO

![](/screenshots/adconnect/sso.png) 

- Configuration completed  

![](/screenshots/adconnect/configcomplete.png) 

#Conclusion#  

- Created users on Domain Controller  

![](/screenshots/adconnect/createdusers.png) 

- Verified users synced to Azure AD after 30 minutes  

![](/screenshots/adconnect/azureadusers.png) 

- Assigned O365 license to test1 account  

![](/screenshots/adconnect/o365license.png) 

- Joined Windows 10 client to domain  

![](/screenshots/adconnect/win10joined.png) 

- Logged in with test1 account  

![](/screenshots/adconnect/win10loggedin.png) 

- O365 license applied successfully  

![](/screenshots/adconnect/o365working.png) 

###Final Thoughts###  
This lab provided me an avenue to test the synchronization between Azure AD and an On-Premise Active Directory Forest. I ran into three issues during this lab. One during the installation of Azure AD Connect. The second when assigning the O365 license to test1 in Azure AD. And the last when attempting to join the Windows 10 client to the *jordantg.com* domain. 

The issue with Azure AD Connect was related to TLS 1.2 not being enabled before installation.  

![](/screenshots/adconnect/tlserror.png) 


I resolved this issue by running a powershell script to enable TLS 1.2 on the DC.  

The O365 license issue was related to a usage location needing to be set prior to the license assignment. I was able to edit the user and set the location for test1 to the United States in the Azure portal. 

![](/screenshots/adconnect/setlocation.png) 


The Windows 10 client's issue was related to the client not finding the *jordantg.com* domain controller. I resolved this issue by specifying the DC's (192.168.1.225) ip address as a primary DNS entry on the NIC of the Windows 10 client. 

Overall it was a great introduction providing me high-level exposure to how Azure connects with on-premise environments. The lab also showed me how seamlessly you can apply an O365 license from the Azure portal (given you have available licenses, I had 25 trial licenses from creating a new subscription account). 







