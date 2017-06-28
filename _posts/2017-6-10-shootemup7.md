---
layout: post
title:  "Shoot 'em up tutorial: 7 - Score, spawning"
date:   2017-6-10 18:49:11 +0200
author: arcadio
categories: tutorial shootemup
heropic: /pictures/shootemup0.png
---

Now we only need to find a way to spawn them in waves instead of all at once. We are going to write a component that will take care of that:
{% highlight js %}
{
        name: "enemySpawner",
        events: [
            {
                name: "#setup", code: function (event) {
                    this.var.timeline = [
                        { enemy: "kamikazeShip", x: 600, t: 70 },
                        { enemy: "kamikazeShip", x: 200, t: 200 },
                        { enemy: "kamikazeShip", x: 900, t: 200 },
                        { enemy: "kamikazeShip", x: 200, t: 500 },
                        { enemy: "kamikazeShip", x: 900, t: 600 },
                        { enemy: "kamikazeShip", x: 200, t: 700 },
                        { enemy: "cannonShip", x: 600, t: 850 },
                        { enemy: "cannonShip", x: 600, t: 1000 },
                        { enemy: "kamikazeShip", x: 200, t: 1050 },
                        { enemy: "kamikazeShip", x: 900, t: 1050 },
                        { enemy: "cannonShip", x: 200, t: 1120 },
                        { enemy: "cannonShip", x: 900, t: 1120 },
                        { enemy: "kamikazeShip", x: 100, t: 1200 },
                        { enemy: "kamikazeShip", x: 300, t: 1200 },
                        { enemy: "kamikazeShip", x: 700, t: 1200 },
                        { enemy: "kamikazeShip", x: 1100, t: 1200 },
                        { enemy: "triangleShip", x: 600, t: 1300 },
                        { enemy: "triangleShip", x: 200, t: 1400 },
                        { enemy: "triangleShip", x: 900, t: 1400 },
                        { enemy: "kamikazeShip", x: 100, t: 1410 },
                        { enemy: "kamikazeShip", x: 100, t: 1410 },
                        { enemy: "kamikazeShip", x: 1100, t: 1410 },
                        { enemy: "kamikazeShip", x: 600, t: 1410 },
                        { enemy: "cannonShip", x: 200, t: 1120 },
                        { enemy: "cannonShip", x: 900, t: 1120 },
                    ];
                    this.var.timer = 0;
                }
            },
            {
                name: "#loop", code: function (event) {
                    var that = this;
                    this.var.timeline.filter(function (event) {
                        return event.t == that.var.timer;
                    }).forEach(function (event) {
                        that.engine.spawn("spawnedEnemy", event.enemy, { $x: event.x, $y: -100, $z: 0 });
                    });
                    this.var.timer++;
                }
            }
        ]
    }
{% endhighlight %}

It has no spritesheet, since it will be invisible, but is has stored the list of enemies, with the position and time at which they should be spawned. Then every frame, it will update the timer, and filter that list to the ones corresponding to the current time, and for each of them (none in most of the frames), it will spawn the specified enemy at the specified x coordinate. This enemy list is an example, make your own to design unique levels!

In the levels file, delete the three enemy ships and replace them by the enemy spawner:
{% highlight xml %}
      <object name="enemySpawner" type="enemySpawner" x="0" y="0" ></object>
          {% endhighlight %}


Now try your game and try to make your way through your own level. We are almost done! The last thing we are going to add is a score counter so the player can brag about how many extraterrestrial enemies they have blasted. Its spritesheet will be the following:

{% highlight xml %}
  <spritesheet name="score">
    <states>
      <state name="score">
        <layer name="score"></layer>
      </state>
    </states>
    <layers>
      <layer name="score" x="0" y="0">
        <frame name="score"></frame>
      </layer>
    </layers>
    <frames>
      <frame name="score" code="context.font='70px Verdana';context.fillStyle='white';context.fillText(vars['$score'],x,y);"></frame>
    </frames>
  </spritesheet>
      {% endhighlight %}


As you can see, instead of using a picture as a source, we are using a code frame, which will execute the specified instructions. In this case, it will draw the text found in the $score variable at the specified position.
Its component will be like this:

{% highlight js %}
    {
        name: "score",
        sprite: "score",
        events: [
            {
                name: "#setup", code: function (event) {
                    this.var.$score = 0;
                }
            },
            {
                name: "scorePoints", code: function (event) {
                    this.var.$score += event;
                }
            }
        ]
    }
        {% endhighlight %}


It listens to the scorePoints event produced by the enemies when they die and updates the score. Finally, let's add it to the level:
{% highlight xml %}
      <object name="score" type="score" x="60" y="100" ></object>
          {% endhighlight %}


 
Deploy your game and everything should work! Congratulations, you just made your first Clockwork game! Now you could keep adding spritesheets and components to improve the gameplay, import more dependencies such as support for gamepad or touch screens, or even change the rendering library to develop a VR game.  You should definitely try using the Web and UWP bridges to export your game as a website or an app.

If you are still reading, thanks for your patience! We hope this tutorial gave you a better understanding of our platform, taught you how to start developing your own games and, most importantly, you had some fun!
