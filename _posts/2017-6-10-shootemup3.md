---
layout: post
title:  "Shoot 'em up tutorial: 3 - Keyboard controls"
date:   2017-6-10 18:47:11 +0200
author: arcadio
categories: tutorial shootemup
heropic: /pictures/shootemup0.png
---

Let’s add keyboard support to your game!

Since you are now a game developer, you must follow the golden rule of development: do as little work as possible! That is why instead of dealing with keyboard input yourself, we are just going to find a package that does the work for you! (Seems like a good deal, doesn’t it?)

Press Ctrl+` inside VS Code to open a terminal, and execute this command:

`clockwork add keyboard`

It will output

`Version 1.0 of keyboard added to the dependencies`

Let’s find out what happened! If you go to `manifest.json`, you will find that someone touched your code: it is now one long messy line! First of all, you should press `Shift+Alt+F` to make it more readable and avoid hurting your eyes. Immediately after, you will notice something new:

{% highlight js %}
"dependencies": {
        "keyboard": "1.0"
    },
{% endhighlight %}

A dependency has been added to the project! This will tell the Clockwork Runtime to add that piece of code to your game automatically. This seems really helpful, but we still don’t know how to use it! There is an easy solution, press F1 and execute the command `Browse Clockwork package documentation`, this will ask you for the package name (keyboard) and the version (1.0), and then will open the documentation for that package. By the way, that documentation is automatically generated, so it will follow the same standard style for any package. Handy, isn’t it?

Anyway, if you read it you will discover that it lets you use a new component, aptly named keyboard. So, let’s go to `levels.xml` and add it to the current level, just after the playerShip:

{% highlight xml %}
    <object name="keyboard" type="keyboard" x="0" y="0" ></object>
{% endhighlight %}

If you try deploying your game now, you will see no changes. Why is it so? Sadly, we haven’t yet discovered how to read minds and predict the future (we would like it to incorporate to our roadmap though!), so we don’t know how do you want to use that keyboard input, so you will have to do a small effort. If you read the documentation, you will see that it triggers two new events, `keyboardDown` and `keyboardUp,` which conveniently tell you which key has been pressed/lifted. The only issue is that it talks about something called the key code, and we don’t know which key code correspond to each key. While a quick Bing search would reveal the answer, we are going to find out by ourselves as an excuse to try more cool Clockwork features.

Change the playerShip component so it looks like this:

{% highlight js %}
{
        name: "playerShip",
        sprite: "playerShip",
        inherits: "ship",
        events: [
            {
                name: "keyboardDown", code: function (event) {
                    this.engine.debug.log(event.key);
                }
            }
        ]
    }
{% endhighlight %}

This will print in the debug log the code of each letter pressed. Now deploy your game again but, instead of clicking on its tile, go back to VS Code and press F5. Now go to the game and feel free to mash your keyboard (please don’t hold us accountable for any physical damage you inflict to your keyboard).  You will see the key codes for each letter being outputted in the debug console.

![The output in the debug log console]({{ site.baseurl }}/pictures/shootemup3.png)

As you can imagine, the debug console is very useful for keeping track of what is happening inside your game and easily find bugs. Clockwork itself will also report common errors in your game, but remember, you must be debugging the game (F5) to see them!

Now replace that keyboardDown event by these two:

{% highlight js %}
{
                name: "keyboardDown", code: function (event) {
                    switch (event.key) {
                        case 37:
                            this.var.ax = -1;
                            break;
                        case 38:
                            this.var.ay = -1;
                            break;
                        case 39:
                            this.var.ax = 1;
                            break;
                        case 40:
                            this.var.ay = 1;
                            break;
                        case 32:
                            this.do.fire();
                            break;
                    }
                }
            },
            {
                name: "keyboardUp", code: function (event) {
                    switch (event.key) {
                        case 37:
                            this.var.ax = 0;
                            break;
                        case 38:
                            this.var.ay = 0;
                            break;
                        case 39:
                            this.var.ax = 0;
                            break;
                        case 40:
                            this.var.ay = 0;
                            break;
                    }
                }
            }

{% endhighlight %}

This is all the code you will need to process keyboard input, basically you will modify the acceleration when a key is pressed, and reset it when it is released. You will notice we are also introducing a new feature when the space bar (key code 32) is pressed. `this.do.eventname()` instructs the current object to execute that event, but since we have not yet defined the fire event this will do nothing.

Let’s see what we got! Deploy your game and you will be able to control your game using the keyboard.

In [part 4 of this tutorial]({{ site.baseurl }}{% post_url 2017-6-10-shootemup4 %}) we will learn how to add some firepower to your ship!