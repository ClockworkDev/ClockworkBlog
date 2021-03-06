---
layout: post
title:  "The boxBoxCollision2D package"
date:   2017-6-10 19:47:11 +0200
author: arcadio
categories: collisions packages
heropic: /pictures/ruler.jpg
---

The `boxBoxCollision2D` package registers a collision detector which will enable you to detect when a object shaped like a 2D box overlaps with other object shaped like a 2D box.

First of all, you will need to add the package as a dependency to your game. You can do it using the command line `clockwork-tools`, opening a command line in your project folder and typing

`clockwork add boxBoxCollision2D`

Once you have added the dependency, you need to modify the components whose collisions do you want to detect:

{% highlight js %}
    {
        name: "boxShapedObject1",
        events:[
            {
              name: "#collide", code: function (event) {
                  //Do something
              }
            }
        ],
         collision: {
            "box": [
                { "x": 0, "y": 0, "w": 100, "h": 100, "#tag": "nameOfTheBox" },
            ]
        }
    },
    {
        name: "boxShapedObject2",
        events:[
            {
              name: "#collide", code: function (event) {
                  //Do something
              }
            }
        ],
         collision: {
            "box": [
                { "x": 0, "y": 0, "w": 100, "h": 100, "#tag": "nameOfTheBox" },
            ]
        }
    }
{% endhighlight %}

Just like that, you can add as many boxes and points to the components as you like own logic to detect keyboard input and act acordingly.

Each box has a unique identifier (`#tag`),  a position (x,y values) which will be realtive to the object position, and a width and height specified.

Finally, remember that if you need to remember something about how the component is used, you can quickly access the package documentation from Visual Studio Code, running the `Browse Clockwork package documentation` command.