---
layout: post
title:  "Shoot 'em up tutorial: 4 - Projectiles"
date:   2017-6-10 18:48:11 +0200
author: arcadio
categories: tutorial shootemup
heropic: /pictures/shootemup0.png
---

It doesn’t matter if your ship is able to make the Kessel Run in less than 12 parsecs if it is unable to defend against hordes of villainous aliens, so the next step will be to add some firepower.
First, let's add this spritesheet that defines some nice bright red projectiles:

{% highlight xml %}
  <spritesheet name="playerFire" src="images/spaceships.png">
    <states>
      <state name="Idle">
        <layer name="Idle"></layer>
      </state>
    </states>
    <layers>
      <layer name="Idle" x="-50" y="-30">
        <frame name="Idle1"></frame>
        <frame name="Idle2"></frame>
        <frame name="Idle1"></frame>
        <frame name="Idle3"></frame>
      </layer>
    </layers>
    <frames>
      <frame name="Idle1" x="400" y="0" w="100" h="100" t="50"></frame>
      <frame name="Idle2" x="400" y="100" w="100" h="100" t="50"></frame>
      <frame name="Idle3" x="400" y="200" w="100" h="100" t="50"></frame>
    </frames>
  </spritesheet>
{% endhighlight %}


Now, add this component to the list:
{% highlight js %}
{
        name: "playerFire",
        sprite: "playerFire",
        events: [
            {
                name: "#loop", code: function (event) {
                    this.var.$y -= 10;
                    if (this.var.$y < -50) {
                        this.engine.destroy(this);
                    }
                }
            },
            {
                name: "#collide", code: function (event) {
                    this.engine.destroy(this);
                }
            }]
    }
{% endhighlight %}
    
As you can see, it will use the desired spritesheet, and will move upwards 10 pixels each frame. When its position is 50 px above the upper edge of the screen, it will delete itself using the `this.engine.destroy` function. If you have not guessed it yet, methods of `this` interact with the current object, while methods of  `this.engine` interact with the whole level. It will also delete itself if it collides with something, but we will forget about that for now. Then, add the fire event to the list of events of playerShip:
{% highlight js %}
{
                name: "fire", code: function (event) {
                    if (Math.random() > 0.5) {
                        this.engine.spawn("somePlayerFire", "playerFire", { $x: this.var.$x + 40, $y: this.var.$y, $z: this.var.$z - 1 });
                    } else {
                        this.engine.spawn("somePlayerFire", "playerFire", { $x: this.var.$x + 70, $y: this.var.$y, $z: this.var.$z - 1 });
                    }
                }
            }
{% endhighlight %}
            
It will randomly shoot from either the left or right cannon, spawning a `playerFire` called somePlayerFire at the desired relative position. Deploy your game and press the space bar to see some nice fireworks!

You can continue to [part 5 of this tutorial]({{ site.baseurl }}{% post_url 2017-6-10-shootemup5 %}), where you will learn how to add some enemies.