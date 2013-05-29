---
layout: tutorial
title: Handling Collision
category: basic
---

If you've read the [FlashPunk Basics][1] and [Keyboard & Mouse Input][2] tutorials, you're familiar with making an Entity appear and move around with the arrow keys. But one of the most useful things to be able to do in a game is make Entities interact with each other in different ways, and FlashPunk has a set of useful collision functions for this purpose that will be explained in this tutorial.

 - Assigning Hitboxes
 - Collision Types
 - Colliding Entities

Assigning hitboxes
--

FlashPunk uses rectangles for all base-level collision between Entities; you define a rectanular collision area for your Entity, called its **hitbox**, and then you can use FlashPunk's functions to test if one Entity's hitbox intersects with anothers'. So if we have an Entity called **Player**, in it we can define hitbox parameters like this:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;

	public class Player extends Entity
	{
		public function Player()
		{
			// Here I set the hitbox width/height with the setHitbox function.
			setHitbox(50, 50);

			// Here I do the same thing by just assigning Player's properties.
			width = 50;
			height = 50;
		}
	}
}
{% endhighlight %}

I set the hitbox values two different ways here. Both ways work the same, so you can do it whichever way you prefer. Now that we have one Entity with a 50×50 hitbox, let's create another Entity to collide with our Player. We'll call our other Entity **Bullet**, and then we'll check to see if Player is colliding with it.

So our Bullet Entity will look like this:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;

	public class Bullet extends Entity
	{
		public function Bullet()
		{
			setHitbox(10, 10);
		}
	}
}
{% endhighlight %}

Collision types
--

So we created the bullet Entity and gave it a 10×10 hitbox, but there's one more line we need to add before we can check if they intersect:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;

	public class Bullet extends Entity
	{
		public function Bullet()
		{
			setHitbox(10, 10);
			type = "bullet";
		}
	}
}
{% endhighlight %}

Here, I assign a [type][3] to the bullet Entity, and give it the value "bullet". What this does is categorizes the object under the "bullet" type in FlashPunk, because when you want to check for collision against another Entity, you have to provide a type to check against.

> Entity types must be a string value, and do not have to share a name
> with their corresponding object. You could have multiple different
> Classes all under the "solid" type, but keep in mind that
> type-checking is case-sensitive (eg. "solid" is not the same as
> "Solid" or "SOLID"). So when a **Bullet** instance is added to the World,
> it will be added to the "bullet" group. In the next step, I'll show
> you how to check if our Player is colliding with a Bullet object by
> using this assigned type.

Colliding entities
--

Now that we have our Bullet Entity classified under the "bullet" group, we can go back to our Player and use Entity's handy [collide()][4] function to check if our Player is intersecting with any instances of the "bullet" type in the World:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;

	public class Player extends Entity
	{
		public function Player()
		{
			setHitbox(50, 50);
		}

		override public function update():void
		{
			if (collide("bullet", x, y))
			{
				// Player is colliding with a "bullet" type.
			}
		}
	}
}
{% endhighlight %}

So here, we use collide() to check if the Player will collide with any "bullet" objects when placed at its current location (x, y). If you wanted to check 10 pixels ahead, you could do so as well:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;

	public class Player extends Entity
	{
		public function Player()
		{
			setHitbox(50, 50);
		}

		override public function update():void
		{
			if (collide("bullet", x + 10, y))
			{
				// Player will intersect a "bullet" at (x + 10, y).
			}
		}
	}
}
{% endhighlight %}

But FlashPunk also supports more specific collision behavior as well. Let's say we created our Bullet with a function to destroy the Entity, removing it from the World, like so:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;
	import net.flashpunk.FP;

	public class Bullet extends Entity
	{
		public function Bullet()
		{
			setHitbox(10, 10);
			type = "bullet";
		}

		public function destroy():void
		{
			// Here we could place specific destroy-behavior for the Bullet.
			FP.world.remove(this);
		}
	}
}
{% endhighlight %}

So now, what if we want the Bullet to be destroyed when the Player collides with it? For this, we can use the collide() function in Player again, and do this:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;

	public class Player extends Entity
	{
		public function Player()
		{
			setHitbox(50, 50);
		}

		override public function update():void
		{
			// Assign the collided Bullet Entity to a temporary var.
			var b:Bullet = collide("bullet", x, y) as Bullet;

			// Check if b has a value (true if a Bullet was collided with).
			if (b)
			{
				// Call the Bullet's destroy() function.
				b.destroy();
			}
		}
	}
}
{% endhighlight %}

This time, instead of putting collide() in the statement, we just assign its return value to a variable. What the collide() function will do is check the World for any intersecting instances of "bullet", and if it finds one, it will return the value of that Entity object. If it doesn't find any intersections, it will return the ActionScript **null** type. With this knowledge, we can then use that assigned variable in the if-statement, since the statement will only evaluate true for a non-null value. Then, I simply use the variable as access to call the Bullet's destroy() function.


  [1]: {% post_url 2001-01-01-flashpunk-basics %}
  [2]: {% post_url 2001-01-02-keyboard-and-mouse-input %}
  [3]: http://useflashpunk.net/docs/net/flashpunk/Entity.html#type
  [4]: http://useflashpunk.net/docs/net/flashpunk/Entity.html#collide%28%29

*Original tutorial by Chevy Ray Johnston*