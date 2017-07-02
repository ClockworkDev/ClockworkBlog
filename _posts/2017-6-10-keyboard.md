---
layout: post
title:  "The keyboard package"
date:   2017-6-10 19:45:11 +0200
author: arcadio
categories: keyboard packages
heropic: /pictures/keyboard.jpg
---

The keyboard is one of the most popular input methods in PC games, and thanks to the `keyboard` package it is really easy to incorporate it into your game.

First of all, you will need to add the package as a dependency to your game. You can do it using the command line `clockwork-tools`, opening a command line in your project folder and typing

`clockwork add keyboard`

Once you have added the dependency, you just need to spawn a `keyboard` object in any level you want to detect keyboard input. Add it to the level like this:

{% highlight xml %}
<object name="keyboard" type="keyboard" x="0" y="0" ></object>
{% endhighlight %}

and you will be ready to start detecting keyboard input!

The `keyboard` object you just created will trigger two events, `keyboardDown` and `keyboardUp`, that you can listen to in order to find out when a key is pressed or released. For example, this would be a component that will move right when a certain key is pressed, and move left when another one is released:

{% highlight js %}
    {
        name: "someObject",
        events: [
            {
                name: "keyboardDown", code: function (event) {
                    if(event.key==65){ // 'A' is pressed
                        this.var.$x++;
                    }
                }
            },
            {
                name: "keyboardUp", code: function (event) { 
                  if(event.key==66){ // 'B' is released
                        this.var.$x--;
                  }
                }
            }
        ]
    }
{% endhighlight %}

Just like that, you can add your own logic to detect keyboard input and act acordingly.

The key code corresponding to each key is the standard one reported by the browser, [here you can find a list of the most key codes](https://gist.github.com/arcadiogarcia/e477a424eb9a4355724fdfed8ad7469b), or you could just log them (via `this.engine.debug.log(event.key)`) and find out the numbers for the keys you are interested in.

Finally, remember that if you need to remember something about how the component is used, you can quickly access the package documentation from Visual Studio Code, running the `Browse Clockwork package documentation` command.