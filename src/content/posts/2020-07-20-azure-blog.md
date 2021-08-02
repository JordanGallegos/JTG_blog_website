---
template: blog-post
title: Migrating Blog to Azure Static Web Pages
slug: /migrating-blog-to-azure-static-web-pages
date: 2021-07-22 12:00
description: azure blog
featuredImage: /assets/microsoft-azure-500x500.png
---

#Purpose#
Merge two separate web pages of the jordantg.com domain into one Gatsby blog hosted on Azure for free using Static Web Pages. Moving to Azure will cut costs as I will no longer have to pay for any AWS hosting. 

###Old topology:###  
1. *jordantg.com* | githubpages gatsby static website  
![](/screenshots/azure-blog/jordantg.png "screenshot of jordantg.com")  
2. *docs.jordantg.com* | Ghost blog in AWS for posting articles, route 53 AWS for sub domain records  

###New topology###
1. *jordantg.com* | Gatsby static site repository from GitHub hosted as an Azure Static Web App 

##Configuration##
I used another Gatsby static site to save me time from having to create my own web page. I chose a Gatsby starter site that allows for a main homepage and a blog implementation  

Here is the link to the static site [https://github.com/stackrole/gatsby-starter-foundation](https://github.com/stackrole/gatsby-starter-foundation)  

- Cloned the git repository and built the Gatsby site locally
- Once the development site (*gatsby develop*) was properly running locally, I updated the static site to my needs
- Next step was to push the Gatsby site to Azure
- Logged into Azure Portal, created a new resource
- Searched for Static Web App, then created new Static Web App  

![](/screenshots/azure-blog/create.png) 
  - Created a new resource group  
  - Named the Static Web App  
  - Chose plan type (free)  
  - Chose source (github)  
  - Selected organization, repository, and branch relating to my repo  
  - Selected Gatsby as build preset![](/screenshots/azure-blog/webappsettings.png "Static Web App settings in Azure Portal")  
- Once the deployment was complete, browsed to the resource to observe the GitHub action run
![](/screenshots/azure-blog/resourcedash.png "Static Web App")  
- Action completed, website live  

![](/screenshots/azure-blog/workflowsuccess.png)  
- Site reachable via Azure web address  

![](/screenshots/azure-blog/wonderful.png)  
- Added Custom Domain from Static Web App  

![](/screenshots/azure-blog/customdomain.png)  
- Set domain name *www.jordantg.com* as a CNAME record on domain provider (namecheap)  

![](/screenshots/azure-blog/cdjordan.png)  
- Set domain name *jordantg.com* as a TXT record on domain provider (namecheap)  

![](/screenshots/azure-blog/cdjordan1.png)  

#Conclusion#

Both *www.jordantg.com* and *jordantg.com* resolve

![](/screenshots/azure-blog/dnsresolve.png)  

###Final Thoughts###
This lab provided me a scenario to gain exposure to Azure. I needed to migrate my domain website and subdomain website off AWS and Azure Static Web Pages was a great resource that allowed me to combine both. The hardships of this migration related to Gatsby and spinning up the development environment locally on my desktop. But once it was properly built, the push out to Azure was seamless. I was able to create an azure portal account, create a resource group for the blog, and create/publish the static web app in less than an hour. 








 






