---
layout: post
title: "Web Bridge"
date: 2017-7-2 19:49:11 +0200
author: silvia
categories: bridges tutorial
heropic: /pictures/azureweb.PNG
---


To allow developers to publish their games in different platforms, Clockwork offers a set of bridges, programs that will export Clockwork games (.cw) to native apps. One of these is the Web Bridge, which generates websites that host the games so they can be played in any browser.

###Generating the web
In order to use this bridge, open your game's folder in Visual Studio Code and press F1 and write `Export game using the Web Bridge`. Clockwork will then create a folder named `web` for you. Inside it, you will find everything needed to host your game in a website.
If you want to give it a try locally you might want to create a fast local server. You can do this with Node.js very easily. Install [Node.js](https://nodejs.org/en/) and then use npm to install [http-server](https://www.npmjs.com/package/http-server). Open cmd in the folder where you want to test it (the web folder here, where the index is) and run `http-server`. Now open a browser and go to `localhost:8080` and your game will be there.

###Hosting your game in Azure
Now you probably want to host that web somewhere. We do it with Microsoft Azure. You can get Azure through [Visual Studio Dev Essentials](https://azure.microsoft.com/en-us/pricing/member-offers/vs-dev-essentials/) or [Dreamspark](https://azure.microsoft.com/en-us/pricing/member-offers/imagine/?cdn=disable) if you are a student.

When you have Azure, open the portal at `portal.azure.com` and create a new Web App by clicking `New` at the left menu and selecting Web + Mobile > Web App. Give a name to your web for the URL and choose the subscription, resource group, OS, and service plan to use. If you don't know what to choose, just leave the defaults and create a new resource group with the name of your game. :) I also recommend to click the "Pin to dashboard" checkbox at the bottom right above the `Create` button so you can find it easily when using the Azure portal. Now click `Create`!

The website is now ready but it is still empty. You can visit it and see it tells you "Your App Service app has been created". To put your game inside this website you have a few methods available. I myself am a big fan of deploying from Github, or Onedrive for fast things, so I'll cover that. 


####Github deployment
This method requires you to use Github, obviously. It isn't very complex though, so you might want to try it. From the overview of your new Web App, search for `Deployment options` in the left menu, click it and then click on `Choose Source`. Choose Github and Azure will ask you for your Github credentials. It will then ask you to choose a project from Github and a branch from which to deploy. It is a good practice to have a `dev` branch in your project to develope and leave the `master` branch for the deployment, committing to it only working versions. And that is all, it might take a few minutes to sync but, after that, you can check your website again and you'll see the game running!


####Onedrive deployment
This method can be a bit messy but it is easy and very fast. From the overview of your new Web App, search for `Deployment options` in the left menu, click it and then click on `Choose Source`. Choose Onedrive and most of the options will be already filled in. Azure will create a folder with the name of the web app in your Onedrive. 

Go to Onedrive and you'll probably see a new folder named "Applications" and "Azure Web Apps" inside it. There you have the folder that will be synced. Fill it with the contents of the web folder created by Clockwork so that the index.html is in the root. Include there too [this Web.config file](https://gist.github.com/arcadiogarcia/90915843d14d53459148d77a630b93c0) that makes Azure allow json files.

Go back to the Azure portal and back to `Deployment options` and click on `Sync`. When that's ready, go visit your webpage again and the game will be running!