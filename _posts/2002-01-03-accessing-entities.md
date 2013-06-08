---
layout: tutorial
title: Accessing Entities
category: intermediate
---

One of the most common problems new ActionScript programmers come across when developing games is having variables accessible where they need to be and communicating information between game objects. In this tutorial, we'll take a look at some of FlashPunk's available tools for dealing with this problem.

## Contents

<ul class="nav nav-pills nav-stacked">
	<li><a href="#finding-unique-entities">1. Finding Unique Entities</a></li>
	<li><a href="#finding-multiple-entities">2. Finding Multiple Entities</a></li>
</ul>

<h2 id="finding-unique-entities">Finding Unique Entities</h2>

Sometimes you need a particular entity to be available for all other entities to interact with. For example, you might want all instances of an Enemy class to constantly move towards the Player. While you could use a [static][] variable to achieve this, using static variables is often a bad idea. Fortunately, FlashPunk has you covered.

The Entity class has a property called [name][entity-name] that allows you to find specific instances of entities in the World with the [getInstance()][world-getInstance] function.

{% highlight actionscript %}
public class Player extends Entity
{
	public function Player()
	{
		name = "player";
	}
}
{% endhighlight %}
Now, whenever you want to access your Player instance, you can do this:

{% highlight actionscript %}
var player = world.getInstance("player") as Player;

if (!player)
{
	// the player hasn't been added to the world yet!
}
{% endhighlight %}

If you're going to be calling this function a lot, it's a good idea to store the result in a variable to improve performance.

<h2 id="finding-multiple-entities">Finding Multiple Entities</h2>

FlashPunk has other ways of getting information from other Entities, and several useful functions to simplify the process of communicating between Entities. FlashPunk's [World][] class is pumped full of useful functions for fetching Entities by class, type, or layer.

For example, say you're creating a scrolling shooter game and your character picks up a bomb powerup; when this powerup is collected, you want to destroy all the enemies in the World. This is where FlashPunk comes in handy! In your bomb object, you could use the following code to find them all:

{% highlight actionscript %}
public class Bomb extends Entity
{
	public function Bomb()
	{

	}

	public function explode():void
	{
		
		// First, we will create an empty array.
		var enemyList:Array = [];

		// Then, we populate the array with all existing Enemy objects!
		world.getClass(Enemy, enemyList);

		// Finally, we can loop through the array and call each Enemy's die() function.
		for each (var e:Enemy in enemyList)
		{
			e.die();
		}
		
	}
}
{% endhighlight %}

Here, we used World's [getClass()][world-getClass] function and told it to fetch all Enemy entities in the world; by passing it the enemyList array, we're telling the function to fill that array with all the Enemy objects so that we can use them in the for-each loop below. So as a result, when Bomb's explode() function is called, it will then call the die() function of every Enemy!

You could easily add more conditions inside the for-each loop too, such as checking to make sure the enemy is close enough to the Bomb (to give the bomb an explosion radius), or checking it's current state (maybe blinking enemies cannot be bombed, etc.) Regardless, this method does not always suffice, so you can use World's other similar fetching functions as well:

 - [`getClass()`][world-getClass] &rarr; gets all Entities by Class
 - [`getType()`][world-getType] &rarr; gets all Entities by Type (faster than getClass!)
 - [`getLayer()`][world-getLayer] &rarr; gets all Entities by layer
 - [`classFirst()`][world-classFirst] &rarr; returns the first Entity of the Class found
 - [`typeFirst()`][world-typeFirst] &rarr; returns the first Entity of the type found
 - [`layerFirst()`][world-layerFirst] &rarr; returns the first Entity on the layer
 - [`nearestToEntity()`][world-nearestToEntity] &rarr; returns the Entity of a type nearest to another
 - [`nearestToPoint()`][world-nearestToPoint] &rarr; returns the Entity nearest to the position
 - [`nearestToRect()`][world-nearestToRect] &rarr; returns the Entity nearest to the rectangular area

These functions should provide lots of help when communicating Entities in your games, but if you ever have more involved or specific questions regarding this topic, don't be afraid to drop by the [community][] and ask for assistance!

[static]: http://help.adobe.com/en_US/ActionScript/3.0_ProgrammingAS3/WS5b3ccc516d4fbf351e63e3d118a9b90204-7f31.html
[entity-name]: http://useflashpunk.net/docs/net/flashpunk/Entity.html#name
[world]: http://useflashpunk.net/docs/net/flashpunk/World.html
[world-getInstance]: http://useflashpunk.net/docs/net/flashpunk/World.html#getInstance%28%29
[world-getClass]: http://useflashpunk.net/docs/net/flashpunk/World.html#getClass%28%29
[world-getType]: http://useflashpunk.net/docs/net/flashpunk/World.html#getType%29
[world-getLayer]: http://useflashpunk.net/docs/net/flashpunk/World.html#getLayer%28%29
[world-classFirst]: http://useflashpunk.net/docs/net/flashpunk/World.html#classFirst%28%29
[world-typeFirst]: http://useflashpunk.net/docs/net/flashpunk/World.html#typeFirst%28%29
[world-layerFirst]: http://useflashpunk.net/docs/net/flashpunk/World.html#layerFirst%28%29
[world-nearestToEntity]: http://useflashpunk.net/docs/net/flashpunk/World.html#nearestToEntity%28%29
[world-nearestToPoint]: http://useflashpunk.net/docs/net/flashpunk/World.html#nearestToPoint%28%29
[world-nearestToRect]: http://useflashpunk.net/docs/net/flashpunk/World.html#nearestToRect%28%29
[community]: http://developers.useflashpunk.net/category/help
