---
template: blog-post
title: Migrating Blog to Azure Static Web Pages
slug: /migrating-blog-to-azure-static-web-pages
date: 2021-07-20 12:00
description: azure blog
featuredImage: /assets/microsoft-azure-500x500.png
---

#Purpose#
Merge two separate web pages of the jordantg.com domain into one Gatsby blog hosted on Azure for free using Static Web Pages. Moving to Azure will cut costs as I will no longer have to pay for any AWS hosting. 

####Old topology:####  
1. *jordantg.com* | githubpages gatsby static website  
![](/screenshots/jordantg.png "screenshot of jordantg.com")
2. *docs.jordantg.com* | Ghost blog in AWS for posting articles, route 53 AWS for sub domain records  

####New topology####
1. *jordantg.com* | Gatsby static site repository from GitHub hosted as an Azure Static Web App 

##Setup##
I used another Gatsby static site to save me time from having to create my own web page. I chose a Gatsby starter site that allows for a main homepage and a blog implementation  

Here is the link to the static site [https://github.com/stackrole/gatsby-starter-foundation](https://github.com/stackrole/gatsby-starter-foundation)  

Cloned the git repository and built the Gatsby site locally  
Once the development site (*gatsby develop*) was properly running locally, I updated the static site to my needs  

Next step was to push the Gatsby site to Azure

Logged into Azure Portal, created a new resource  

Searched for Static Web App, then created new Static Web App
![](/screenshots/create.png)
 -created a new resource group  
 -named the Static Web App  
 -chose plan type (free)  
 -chose source (github)  
 -selected organization, repository, and branch relating to my repo  
 -selected Gatsby as build preset![](/screenshots/webappsettings.png "Static Web App settings in Azure Portal")

 Once the deployment was complete, browsed to the resource to observe the GitHub action run
 ![](/screenshots/resourcedash.png "Static Web App")

 Action completed, website live  
 
 ![](/screenshots/workflowsuccess.png)




 






