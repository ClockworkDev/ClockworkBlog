---
layout: post
title:  "Shoot 'em up tutorial: 6 - Health system"
date:   2017-6-10 18:49:11 +0200
author: arcadio
categories: tutorial shootemup
heropic: /pictures/shootemup0.png
---

We are going to start adding a spritesheet that will display the number of lives you have left:
{% highlight xml %}
    <spritesheet name="livesIndicator" src="images/spaceships.png">
    <states>
      <state name="3lives">
        <layer name="live1"></layer>
        <layer name="live2"></layer>
        <layer name="live3"></layer>
      </state>
      <state name="2lives">
        <layer name="live1"></layer>
        <layer name="live2"></layer>
      </state>
      <state name="1lives">
        <layer name="live1"></layer>
      </state>
    </states>
    <layers>
      <layer name="live1" x="0" y="0">
        <frame name="ship"></frame>
      </layer>
      <layer name="live2" x="-100" y="0">
        <frame name="ship"></frame>
      </layer>
      <layer name="live3" x="-200" y="0">
        <frame name="ship"></frame>
      </layer>
    </layers>
    <frames>
      <frame name="ship" x="0" y="0" w="100" h="100" t="100"></frame>
    </frames>
  </spritesheet>
  {% endhighlight %}


And writing the corresponding component:

{% highlight js %}
    {
        name: "livesIndicator",
        sprite: "livesIndicator",
        events: [
            {
                name: "#setup", code: function (event) {
                    this.var.lives = 3;
                    this.var.$state = this.var.lives + "lives";
                }
            },  
            {
                name: "playerDamaged", code: function (event) {
                    this.var.lives--;
                    if (this.var.lives > 0) {
                        this.var.$state = this.var.lives + "lives";
                    } else {
                        this.engine.loadLevel("mainLevel");
                    }
                }
            }]
    }
    {% endhighlight %}

When the player runs out of lives, it will call `this.engine.loadLevel` to reload the level. In a full game, you would load a different level containing a game over screen or the main menu.

We will also need to add a collision event to `playerShip`:

{% highlight js %}

     {
                name: "#collide", code: function (event) {
                    this.engine.spawn("explosion", "explosion", { $x: this.var.$x, $y: this.var.$y, $z: this.var.$z + 1 });
                    this.engine.do.playerDamaged();
                }
            }
{% endhighlight %}

And finally add the livesIndicator to the level:

{% highlight xml %}
      <object name="livesIndicator" type="livesIndicator" x="1200" y="50" ></object>
{% endhighlight %}

 Deploy your game again and you will see that you can die under the blast of alien spaceships. Yay!

The game is almost done, keep reading [part 7 of this tutorial]({{ site.baseurl }}{% post_url 2017-6-10-shootemup7 %}) to add the enemy spawner and the score!
