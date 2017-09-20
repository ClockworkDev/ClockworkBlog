---
layout: post
title: "Clockwork: an open platform for HTML5 games"
date: 2017-9-20 1:00:00 +0200
author: arcadio
categories: news
heropic: /img/clockwork.png
---

I'm pleased to announce the official release of [Clockwork, an open platform for developing HTML5 games based on modular components](http://clockwork.js.org/).

# Why Clockwork

Before getting into details about it, let's address the elephant in the room: *Why do we need yet another game engine*? The best way of addressing that question is listing the objectives of the project:

  - Develop a platform that is not only open source but designed from the ground up to be more **open** (as opossed to more traditional 'monolithical' game engines), so you choose / build your own rendering library, physics, native API wrappers...
  - Provide an end-to-end **developer-friendly** experience, with tools that ease common tasks such as deploying, debugging, managing dependencies, publishing... , instead of just providing a .js library and asking the developer to bring their own build tools.
  - Expose an **elegant**, small (and easy to learn!) API, designed around message passing, that makes writing game logic a breeze, and also provide an easy way to use composition and inheritance.
  - Rather than trying to minimize the amount of code that has to be writen by the developer, its top priority should be to maximize the **flexibility** of the engine.
  - Build the platform exclusively on top of web technologies so it can be **easily ported** to any platform and adapted to any workflow.

# How to get started

Want to give Clockwork a test ride? Great! First you will need to [get the tools](http://clockwork.js.org/getstarted.html) and then you can [follow a tutorial]({{ site.baseurl }}{% post_url 2017-6-10-shootemup1 %}) to get started!

# Future work

These are the work items being considered for future releases:

  - [Integrate the package manager commands into the VS extension](https://github.com/ClockworkDev/ClockworkVSCodePlugin/issues/6)

  - [Write a Babylon.js rendering library adapter](https://github.com/ClockworkDev/OfficialClockworkPackages/issues/2)

  - [Export games as PWA (add manifest, service worker)](https://github.com/ClockworkDev/ClockworkWebBridge/issues/2)

  - [Add solid debugging support](https://github.com/ClockworkDev/ClockworkCore/issues/4)

  - [Create a web interface for browsing the package manager](https://github.com/ClockworkDev/ClockworkPackageManager/issues/1)

  - [Add performance tools](https://github.com/ClockworkDev/ClockworkCore/issues/3)


# How to contribute

### Publish your own packages

[If you write a package, you can publish it to the package repository!]({{ site.baseurl }}{% post_url 2017-7-2-packageManager %})

### Contribute to the platform

You are more than welcome to head to [our Github page](https://github.com/ClockworkDev) and start forking repos, opening issues and sending PRs.

In case you would like to get involved but don't know where to start,here are some work items where help would be appreciated:

  - [Port the Clockwork Runtime to other platforms (macOS, linux)](https://github.com/ClockworkDev/ClockworkRuntime/issues/1)
  
  - [Build an Apache Cordova bridge](https://github.com/ClockworkDev/ClockworkWebBridge/issues/1)

# The future

Clockwork was originally born as a side project, with the only goal to provide a lightweight and versatile way to ease up the main pain points I found while doing HTML5 game development. It succeeded at that, and after seeing that it was also helpful to other people I decided to realease it. Where does it go from here? 

My intention is to keep working on it, mainly improving the tooling and adding support for more rendering libraries (aka what you just read on the Future work section), and use it to keep making my own games.

However, I also hope that some of the people reading this post (that's you!) decide to give it a try, some of them provide valuable feedback, and some of them even contribute to the project with their own ideas and code, so we can together keep empowering HTML5 gamedevs to make awesome projects!