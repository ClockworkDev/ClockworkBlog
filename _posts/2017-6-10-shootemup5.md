---
layout: post
title:  "Shoot 'em up tutorial: 5 - Enemies"
date:   2017-6-10 18:49:11 +0200
author: arcadio
categories: tutorial shootemup
heropic: /pictures/shootemup0.png
---

After witnessing the firepower of your fully armed and operational battleship, it is time to give it something to shoot at!  We are going to add some enemies and, as always, we are going to start adding these spritesheets to `spritesheets.xml`:

{% highlight xml %}
    <spritesheet name="cannonShip" src="images/spaceships.png">
    <states>
      <state name="Idle">
        <layer name="Idle"></layer>
      </state>
    </states>
    <layers>
      <layer name="Idle" x="0" y="0">
        <frame name="Idle"></frame>
      </layer>
    </layers>
    <frames>
      <frame name="Idle" x="200" y="0" w="100" h="100" t="100"></frame>
    </frames>
  </spritesheet>
  <spritesheet name="triangleShip" src="images/spaceships.png">
    <states>
      <state name="Idle">
        <layer name="Idle"></layer>
      </state>
    </states>
    <layers>
      <layer name="Idle" x="0" y="0">
        <frame name="Idle"></frame>
      </layer>
    </layers>
    <frames>
      <frame name="Idle" x="300" y="0" w="100" h="100" t="100"></frame>
    </frames>
  </spritesheet>
    <spritesheet name="kamikazeShip" src="images/spaceships.png">
    <states>
      <state name="Health3">
        <layer name="Health3"></layer>
      </state>
      <state name="Health2">
        <layer name="Health2"></layer>
      </state>
      <state name="Health1">
        <layer name="Health1"></layer>
      </state>
    </states>
    <layers>
      <layer name="Health3" x="0" y="0">
        <frame name="Health3"></frame>
      </layer>
      <layer name="Health2" x="Math.random()*8-4" y="Math.random()*8-4">
        <frame name="Health2"></frame>
      </layer>
      <layer name="Health1" x="Math.random()*16-8" y="Math.random()*16-8">
        <frame name="Health1"></frame>
      </layer>
    </layers>
    <frames>
      <frame name="Health3" x="100" y="0" w="100" h="100" t="100"></frame>
      <frame name="Health2" x="100" y="100" w="100" h="100" t="100"></frame>
      <frame name="Health1" x="100" y="200" w="100" h="100" t="100"></frame>
    </frames>
  </spritesheet>

  {% endhighlight %}


We have nicknamed the 3 kind of enemies ‘cannon’, ‘triangle’ and ‘kamikaze’ (Sidequest: try to guess which is which in the spritesheet and then read the coordinates of the frames to check if you were right!). 

The triangle and cannon spritesheets are pretty straight forward, but we are going to use a few tricks in the other: we have three different states which will be used depending on the health of the enemy to display the damage caused to the ship, and when the health is low we are adding random offsets to the position of the layer to show how its warp engine collapses into a singularity  (Pro tip: this is a videogame, so just add cool stuff and then justify it with sciency words!).

Before programming the enemies, we have one problem to solve first: how can we detect when a ship collides with another? Or when a ship collides with a projectile? In order to easily let you solve that problem, Clockwork lets you define collision detectors. These will be functions that decide whether two objects with an specific shape collide, and can be implemented as you want depending on the requirements of your game. For example, you could have a game that happens in a one-dimensional universe, so to check if two objects collide you only need to know if their position in the only axis is the same, but you could also have a game that plays in 4 dimensions (if you manage to imagine that, please let us know, our brains still hurt from trying) or a game where objects collide if they occupy the same position in space and have the same color.

Luckily, we are just going to see if a point is inside a rectangle in the 2D space. Create a file called `collisions.js` inside the `gameFiles` folder and paste this content inside:
{% highlight js %}

CLOCKWORKRT.collisions.register([
    {
        shape1: "player",
        shape2: "enemyFire",
        detector: function (player, enemyFire) {
            if (enemyFire.x >= player.x && enemyFire.y >= player.y && enemyFire.x <= player.x + player.w && enemyFire.y <= player.y + player.h) {
                return true;
            } else {
                return false;
            }
        }
    },
    {
        shape1: "enemy",
        shape2: "playerFire",
        detector: function (enemy, playerFire) {
            if (playerFire.x >= enemy.x && playerFire.y >= enemy.y && playerFire.x <= enemy.x + enemy.w && playerFire.y <= enemy.y + enemy.h) {
                return true;
            } else {
                return false;
            }
        }
    }
]);
{% endhighlight %}

Here we are registering two collision detectors, one for telling if an enemy fire (which will be a point) is inside the player hitbox, and another one for checking the same but for player fire and enemy hitboxes. Actually, you can see that the code for both is the same (replacing the parameter names), so why define two? That way, the engine will only check collisions of enemy fire against the player and not its fellow enemies, and vice versa, meaning it will be more efficient and you won’t need to filter those false positives later. The code itself is very straightforward, it returns true if the point is at the right of the left side of the box, below the top side, at the left of the right side and above the lower side.

Now it is time to let Clockwork know you want to use this code file. Go to the manifest and update the components property:
{% highlight js %}

  "components": [
        "collisions.js",
        "components.js"
    ],
{% endhighlight %}

You can add as many (existent) files as you want, so you could actually divide your components on several files to have your codebase more organized. For the sake of simplicity (and also because we are slightly lazy, though we won’t admit that openly), we will just keep piling components in `components.js`.

Now is where we would tell you to create a component that inherits from ship and adds some logic for the baddies, but we have a problem here. That logic would go inside the `#setup` and `#loop` events, and if you declare them inside your enemy you will overwrite the ones from the parent (`ship`), but we would still like our enemy to follow the laws of physics.

Don’t worry, confused programmer, cause we have you covered! We are going to create a component called `kamikazeLogic`, and then create another one called `kamikazeShip` that inherits from both `kamikazeLogic` and `ship`, so it will inherit both events. This would also potentially allow us to add those “brains” to other objects aside from ships.

This are the components you need to add to `components.js`:
{% highlight js %}

    {
        name: "kamikazeLogic",
        events: [
            {
                name: "#setup", code: function (event) {
                    this.var.ay = 0.5;
                    this.var.friction = 0.1;
                    this.var.health = 3;
                    this.var.$state = "Health" + this.var.health;

                }
            },
            {
                name: "#loop", code: function (event) {
                    var playerShip = this.engine.find("playerShip");
                    if (playerShip.var.$x < this.var.$x) {
                        this.var.ax = -1;
                    } else {
                        this.var.ax = 1;
                    }
                    if (this.var.$y > 800) {
                        this.engine.destroy(this);
                    }
                }
            },
            {
                name: "#collide", code: function (event) {
                    if (event.shape2kind == "player") {
                        this.engine.destroy(this);
                        this.engine.spawn("explosion", "explosion", { $x: this.var.$x, $y: this.var.$y, $z: this.var.$z + 1 });
                    }
                    if (event.shape2kind == "playerFire") {
                        this.engine.spawn("explosion", "explosion", { $x: this.var.$x, $y: this.var.$y, $z: this.var.$z + 1 });
                        this.engine.do.scorePoints(100);
                        this.var.health--;
                        if (this.var.health > 0) {
                            this.var.$state = "Health" + this.var.health;
                        } else {
                            this.engine.destroy(this);
                        }
                    }
                }
            }
        ],
        collision: {
            "enemy": [
                { "x": 0, "y": 0, "w": 100, "h": 100, "#tag": "shipCollision" },
            ],
            "enemyFire": [
                { "x": 50, "y": 100, "#tag": "shipCollision" },
            ]
        }
    },
    {
        name: "kamikazeShip",
        sprite: "kamikazeShip",
        inherits: ["ship", "kamikazeLogic"]
    }
{% endhighlight %}

`KamikazeLogic` will use the setup event to set a permanent acceleration in the y axis, overwrite the friction value to limit its speed and set up some variables of its own. In order to choose which state is used to render the object (from the ones defined in the spritesheet), it sets the value of the `$state` variable. Each frame, it will use `this.engine.find(objectName)` to get a reference to the player and thrust left or right to get closer to it, and when it is past the lower side of the screen it will silently destroy itself.

In the `#collide` event, we write the code that will be executed when it crashes against something. Since this ship might crash against the player in a suicidal attack or be reached by the player fire, we have to differentiate depending on the type of colliding object, found on `shape2kind` (if you want to know more about the information received in the collision events, feel free to have a look at the documentation on the Clockwork website). When it crashes with the player it will destroy itself and display an explosion, but when is reached by the player fire it will add points to the score and decrease its health, updating its state. If its health reaches 0, it will destroy itself.

The most interesting bit is the `collision` property, which sets a collider of type enemy, that defines a bounding box that covers the ship (its coordinates are relative to the object position), and a point of type `enemyfire` in the front of the ship. We are defining two different colliders because we want this ship to behave both as a target for enemy fire and as a bullet itself that can damage the player.

Now we just need to add the explosion spritesheet:

{% highlight xml %}

<spritesheet name="explosion" src="images/spaceships.png">
    <states>
      <state name="explosion">
        <layer name="explosion1"></layer>
        <layer name="explosion2"></layer>
        <layer name="explosion3"></layer>
      </state>
    </states>
    <layers>
      <layer name="explosion1" x="-20" y="-5">
        <frame name="explosion"></frame>
        <frame name="empty"></frame>
        <frame name="empty"></frame>
      </layer>
      <layer name="explosion2" x="5" y="0">
        <frame name="empty"></frame>
        <frame name="explosion"></frame>
        <frame name="empty"></frame>
      </layer>
      <layer name="explosion3" x="0" y="15">
        <frame name="empty"></frame>
        <frame name="empty"></frame>
        <frame name="explosion"></frame>
      </layer>
    </layers>
    <frames>
      <frame name="empty" x="0" y="200" w="100" h="100" t="100"></frame>
      <frame name="explosion" x="0" y="300" w="100" h="100" t="100"></frame>
    </frames>
  </spritesheet>
{% endhighlight %}

And the explosion component:
{% highlight js %}

{
        name: "explosion",
        sprite: "explosion",
        events: [
            {
                name: "#setup", code: function (event) {
                    this.var.timer = 0;
                }
            },
            {
                name: "#loop", code: function (event) {
                    this.var.timer++;
                    if (this.var.timer == 10) {
                        this.engine.destroy(this);
                    }
                }
            }
        ]
    }
{% endhighlight %}

Finally, don’t forget to update the `playerFire` component so it has a playerFire collider:
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
            }],
        collision: {
            "playerFire": [
                { "x": 0, "y": 0, "#tag": "shipCollision" },
            ]
        }
    },
{% endhighlight %}

And add a collider to `playerShip` as well:
{% highlight js %}

   collision: {
            "player": [
                { "x": 0, "y": 0, "w": 100, "h": 100, "#tag": "shipCollision" },
            ]
        }
{% endhighlight %}

And we will now have all the necessary pieces in place, add this object to your level 
{% highlight xml %}

      <object name="kamikazeShip" type="kamikazeShip" x="200" y="0"></object>
      {% endhighlight %}


Deploy your game and try to destroy the ship before it reaches you!
 
Now we are going to add the other two kinds of enemies. They don’t use any new clockwork features, they are just enemies that can be killed on one shot, don’t damage the enemy on contact and shoot either one or two projectiles once in a while. To speed up things a little, here you have all the components needed to implement them (now you should be able to easily understand what they do!):
{% highlight js %}
    {
        name: "cannonLogic",
        events: [
            {
                name: "#setup", code: function (event) {
                    this.var.ay = 0.2;
                    this.var.friction = 0.1;
                    this.var.timer = 0;
                }
            },
            {
                name: "#loop", code: function (event) {
                    this.var.timer++;
                    if (this.var.timer == 100) {
                        this.var.timer = 0;
                        this.do.fire();
                    }
                }
            },
            {
                name: "fire", code: function (event) {
                    this.engine.spawn("someWaveFire", "waveFire", { $x: this.var.$x, $y: this.var.$y + 100, $z: this.var.$z - 1 });
                }
            },
            {
                name: "#collide", code: function (event) {
                    this.engine.spawn("explosion", "explosion", { $x: this.var.$x, $y: this.var.$y, $z: this.var.$z + 1 });
                    this.engine.do.scorePoints(200);
                    this.engine.destroy(this);
                }
            }
        ],
        collision: {
            "enemy": [
                { "x": 0, "y": 0, "w": 100, "h": 100, "#tag": "shipCollision" },
            ]
        }
    },
    {
        name: "triangleLogic",
        events: [
            {
                name: "#setup", code: function (event) {
                    this.var.ay = 0.2;
                    this.var.friction = 0.1;
                    this.var.timer = 0;
                }
            },
            {
                name: "#loop", code: function (event) {
                    this.var.timer++;
                    if (this.var.timer == 50) {
                        this.var.timer = 0;
                        this.do.fire();
                    }
                }
            },
            {
                name: "fire", code: function (event) {
                    var plasma1 = this.engine.spawn("someWaveFire", "plasmaFire", { $x: this.var.$x + 20, $y: this.var.$y + 100, $z: this.var.$z - 1 });
                    plasma1.var.vx = -1;
                    var plasma2 = this.engine.spawn("someWaveFire", "plasmaFire", { $x: this.var.$x + 80, $y: this.var.$y + 100, $z: this.var.$z - 1 });
                    plasma2.var.vx = 1;
                }
            },
            {
                name: "#collide", code: function (event) {
                    this.engine.spawn("explosion", "explosion", this.var.$x, this.var.$y, this.var.$z + 1);
                    this.engine.do.scorePoints(300);
                    this.engine.destroy(this);
                }
            }
        ],
        collision: {
            "enemy": [
                { "x": 0, "y": 0, "w": 100, "h": 100, "#tag": "shipCollision" },
            ]
        }
    },
    {
        name: "cannonShip",
        sprite: "cannonShip",
        inherits: ["ship", "cannonLogic"]
    },
    {
        name: "triangleShip",
        sprite: "triangleShip",
        inherits: ["ship", "triangleLogic"]
    },
    {
        name: "plasmaFire",
        sprite: "plasmaFire",
        events: [
            {
                name: "#loop", code: function (event) {
                    this.var.$y += 4;
                    this.var.$x += this.var.vx;
                    if (this.var.$y > 800) {
                        this.engine.destroy(this);
                    }
                }
            },
            {
                name: "#collide", code: function (event) {
                    this.engine.destroy(this);
                }
            }],
        collision: {
            "enemyFire": [
                { "x": 0, "y": 0, "#tag": "shipCollision" },
            ]
        }
    },
    {
        name: "waveFire",
        sprite: "waveFire",
        events: [
            {
                name: "#loop", code: function (event) {
                    this.var.$y += 4;
                    if (this.var.$y > 800) {
                        this.engine.destroy(this);
                    }
                }
            }, {
                name: "#collide", code: function (event) {
                    this.engine.destroy(this);
                }
            }],
        collision: {
            "enemyFire": [
                { "x": 50, "y": 100, "#tag": "shipCollision" },
            ]
        }
    }
{% endhighlight %}

And you will also need to add the spritesheets for the enemy fire:
{% highlight xml %}

<spritesheet name="waveFire" src="images/spaceships.png">
    <states>
      <state name="Idle">
        <layer name="Wave1"></layer>
        <layer name="Wave2"></layer>
        <layer name="Wave3"></layer>
      </state>
    </states>
    <layers>
      <layer name="Wave1" x="0" y="-(t%1000)/100">
        <frame name="Idle1"></frame>
        <frame name="Idle2"></frame>
      </layer>
      <layer name="Wave2" x="0" y="-(t%200)/5">
        <frame name="Idle2"></frame>
        <frame name="Idle3"></frame>
      </layer>
       <layer name="Wave3" x="0" y="-(1000-(t%1000))/90">
        <frame name="Idle3"></frame>
        <frame name="Idle1"></frame>
        <frame name="Idle2"></frame>
      </layer>
    </layers>
    <frames>
      <frame name="Idle1" x="500" y="0" w="100" h="100" t="100"></frame>
      <frame name="Idle2" x="500" y="100" w="100" h="100" t="100"></frame>
      <frame name="Idle3" x="500" y="200" w="100" h="100" t="100"></frame>
    </frames>
  </spritesheet>
     <spritesheet name="plasmaFire" src="images/spaceships.png">
    <states>
      <state name="Idle">
        <layer name="Idle"></layer>
      </state>
    </states>
    <layers>
      <layer name="Idle" x="-50" y="-50">
        <frame name="Idle1"></frame>
        <frame name="Idle2"></frame>
        <frame name="Idle1"></frame>
        <frame name="Idle3"></frame>
      </layer>
    </layers>
    <frames>
      <frame name="Idle1" x="600" y="0" w="100" h="100" t="50"></frame>
      <frame name="Idle2" x="600" y="100" w="100" h="100" t="50"></frame>
      <frame name="Idle3" x="600" y="200" w="100" h="100" t="50"></frame>
    </frames>
  </spritesheet>
  {% endhighlight %}


Here you can notice a useful technique, we can use `t` (the number of milliseconds passed inside the current animation) as a variable to calculate the layer position, leading to some interesting effects.

Let’s add the new ships to the level file to spawn them:

{% highlight xml %}
<object name="cannonShip" type="cannonShip" x="500" y="-100"></object>
<object name="triangleShip" type="triangleShip" x="800" y="-200"></object>
{% endhighlight %}


Try deploying the game and seeing how their behavior is defined by their code, each of them behaves differently and thus the player will need to follow a different strategy to defeat them. 

You were probably able to defeat the enemy without taking a single hit, weren’t you? Well, don’t get too confident, remember that we haven’t coded the lives system yet, head to [part 6 of this tutorial]({{ site.baseurl }}{% post_url 2017-6-10-shootemup6 %}) to learn how to implement it!
