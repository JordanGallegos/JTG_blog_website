---
template: blog-post
title: Creating a Shared Mailbox in Exchange Admin Center
slug: /creating-shared-mailbox
date: 2021-08-07 12:00
description: Creating a Shared Mailbox in Exchange Admin Center
featuredImage: /assets/exchange.png
---

#Purpose#

To successfully create two shared mailboxes. One using the Exchange Admin Center. The second using powershell in the Azure cloud shell. Along with the creation of the mailboxes, I will create a test user, assign them permissions, and send test emails. 

###Topology###
- Exchange Admin Center | *jordantg.com* domain  

##Configuration##  

####Exchange Admin Center#### 


Created test mailbox in Exchange Admin Center  

![](/screenshots/exchangelab/addmailbox.png)  

test_shared mailbox successfully created  

![](/screenshots/exchangelab/mailboxcreated.png)  

Managed mailbox permissions  
- Assigning *testformailbox@jordantg.com* to 'read and manage' and 'Send as'  

![](/screenshots/exchangelab/mailboxpermissions.png)  

Added read and manage permissions  

![](/screenshots/exchangelab/readandmanage.png)  

Added send as permissions  

![](/screenshots/exchangelab/sendas.png)   

####PowerShell####

- Enabled EXOPSession in Exchange Admin Center CLI  

```
Connect -EXOPSSession
```

![](/screenshots/exchangelab/exop.png)  

- Created *powershell_test* shared mailbox  

![](/screenshots/exchangelab/createdpowershell.png)   

```
New-Mailbox -Shared -Name "powershell_test" -PrimarySmtpAddress “powershell_test@jordantg.com”
```

![](/screenshots/exchangelab/pstest.png)  

- Granted Full Access permissions to *testformailbox* account  

```
Add-MailboxPermission "powershell_test@jordantg.com" -User "testformailbox@jordantg.com" -AccessRights FullAccess -InheritanceType all –AutoMapping $False 
```

![](/screenshots/exchangelab/permissiontops.png)  

- Granted Send as permissions to *testformailbox* account  

```
Add-RecipientPermission "powershell_test@jordantg.com" -Trustee "testformailbox@jordantg.com" -AccessRights SendAs -confirm:$False 

```

![](/screenshots/exchangelab/sendasallowed.png)  

- Permissions granted to *testformailbox@jordantg.com* for both 'read and manage' and 'Send as'

![](/screenshots/exchangelab/permsgood.png)  

#Conclusion#  

-Logged into *testformailbox* account  

- Clicked 'open another mailbox' 

![](/screenshots/exchangelab/open.png)   

- Entered 'test_shared' into search box and clicked open  

![](/screenshots/exchangelab/open1.png)   

- Test_shared mailbox opened successfully  

![](/screenshots/exchangelab/itopened.png)  

- Sent test email to *testformailbox@jordantg.com*  

![](/screenshots/exchangelab/confirmsend.png)  

- Confirmed receipt of email  
 - *testformailbox@occourts.org* has the ability to send from the *test_shared@occourts.org* mailbox  

![](/screenshots/exchangelab/received.png)  

- Entered 'powershell_test' into search box and clicked open 

![](/screenshots/exchangelab/pstestopened.png) 

- Sent test email to *testformailbox@jordantg.com*  

![](/screenshots/exchangelab/attemptps.png)  

- Confirmed receipt of email  
 - *testformailbox@occourts.org* has the ability to send from the *powershell_test@occourts.org* mailbox   

![](/screenshots/exchangelab/received1.png)  


###Final Thoughts###

This lab served as a great introduction to creating shared mailboxes in O365 Exchange. I was previously well versed in the permission granting process (read/manage and send as) however, I never had done it myself. Going through the Web GUI in Exchange Admin Center was a great process that allowed me to manually walk through the steps to gain familiarity and further cement my knowledge of the process. Ideally, as I demonstrated, in a production environment I can see myself interchangeably switching between the manual Web GUI or PowerShell to create mailboxes. The only issue I ran into was the cmdlets in the Azure cloud shell not working (New-Mailbox). This issue was caused by EXOPSSession not being run. Once I ran the command, the cmdlets ran smoothly. 




