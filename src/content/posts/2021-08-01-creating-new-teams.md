---
template: blog-post
title: Creating a Team in Microsoft Teams Admin Center
slug: /creating-microsoft-teams
date: 2021-08-06 12:00
description: Creating a Team in Microsoft Teams Admin Center
featuredImage: /assets/microsoft-team-2019.png
---

#Purpose#

To successfully create a new Microsoft Team using the Teams Admin Center. Furthermore, I will be creating a Group in O365 Admin Center and creating a Team using  that existing Group. Once created I will confirm the newly created Teams show in the desktop applications. 

###Topology###
- O365 Admin Center | *jordantg.com* domain
- Teams Admin Center | *jordantg.com* domain

##Configuration##

####Teams Admin Center####  

Browsed to the manage teams section in Microsoft Teams Admin Center  

![](/screenshots/teamslab/manageteams.png) 

Added a new team  

![](/screenshots/teamslab/newteam.png) 

Successfully created new team  

![](/screenshots/teamslab/createdteam.jpg) 

Created new test channel  

![](/screenshots/teamslab/testchannel.png) 

####O365 Admin Center####  

Created new test group  

![](/screenshots/teamslab/testgroup.png) 

Assigned myself as an owner  

![](/screenshots/teamslab/assignowner.png) 

Created group email address and checked 'create a team for this group'  

![](/screenshots/teamslab/groupemail.png) 

#Conclusion#

Both Teams created  
- Team created from Teams Admin Center
- Team created from Group creation in O365 Admin Center

![](/screenshots/teamslab/clientverified.png) 

###Final Thoughts###  

This lab provided me an introduction to the various ways to create a Team in an O365 environment. Albeit straightforward, I still found it useful to utilize the Web GUI for each process of this lab. Ideally in the future, I will add to this lab using a script to automate both tasks. The thought process is that if I had to create multiple Teams in a production environment having the ability to script the Teams creation would allow me to be more productive. 


