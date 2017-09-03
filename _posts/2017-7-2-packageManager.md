---
layout: post
title: "Using the package repository"
date: 2017-9-1 18:00:00 +0200
author: arcadio
categories: tutorial packages
heropic: /pictures/box.png
---

Many games share some common functionality that you can (and should!) reuse across projects, and this is where the Clocwork package repository comes in handy: you can easily add components, collision detectors and rendering libraries as dependencies to easily incorporate that existing functionality into your game, and you can also upload youw own packages if you want to share your work with the community!

Here are some instructions:

# Adding a dependency

You can run 

`clockwork list`

on a console to see the full list of published packages. If you want to see all the published versions of the package, you can run `clockwork list <packageName>`. Once you have chosen a package, you can run

`clockwork add <packageName>`

in your project folder in order to add the dependency to the manifest. That command will add the last version of the package, but you can `clockwork add <packageName> <packageVersion>` to choose one manually.

Now you are technically done, and can use the components, collision detectors and rendering libraries as if you had added their source files into your project, but there is one last thing you will find helpful.

Even if you will find [posts about the most popular packages in this blog]({{ site.baseurl }}/packages), you will want quick access to the documentation for a package. In order to do so, when working in your project with Visual Studio code, you can run the `Browse Clockwork package documentation` command, input the name of the package, and a tab will open with the package documentation.

# Publishing a package

In order to publish a package first you will need an account in the repository. You can run

`clockwork register`

in order to create a developer account.

Once you have it, you can run

`clockwork publish`

and you will be prompted for the .js file location, the package name and package version, and your credentials. You can reupload different versions of the package later (the use of semantic versioning is encourage), or even overwrite an already published version if you made a mistake. (Once you publish a package from an account, it will be associated as the owner and only that account will be allowed to upload new versions of that package.)

If you think that your package might be useful to other people, you are welcome to submit it to the [official packages repository](https://github.com/ClockworkDev/OfficialClockworkPackages). While it is not mandatory, it will allow other people to collaborate and you will be invited to write a post about it in this blog!

Once last thing: you can add more attributes to the components so that automatic documentation is generated to your package! The feature is not yet properly documented but we encourage you to take a look at the examples in the [previously mentioned repository](https://github.com/ClockworkDev/OfficialClockworkPackages), and ask us if you need help!