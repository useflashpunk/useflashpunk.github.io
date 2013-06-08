---
layout: tutorial
title: Keyboard &amp; Mouse Input
category: basic
---

This tutorial covers how to make your game objects respond to keyboard and mouse input in FlashPunk. This tutorial assumes that you have covered the [FlashPunk Basics][basics] tutorial, and know how to add Entities to Worlds and assign them graphics.

## Contents

<ul class="nav nav-pills nav-stacked">
	<li><a href="#simple-key-checks">1. Simple Key Checks</a></li>
	<li><a href="#specific-key-states">2. Specific Key States</a></li>
	<li><a href="#defining-controls">3. Defining Controls</a></li>
	<li><a href="#mouse-states">4. Mouse States</a></li>
</ul>

<h2 id="simple-key-checks">Simple Key Checks</h2>

The simplest and quickest way to check for keyboard Input in FlashPunk is to use the state-checking functions provided by the [Input][] class combined with the key-constants found in the [Key][] class. Here is an example Entity object that checks various different keys are pressed down:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;
	import net.flashpunk.utils.Input;
	import net.flashpunk.utils.Key;

	public class MyEntity extends Entity
	{
		public function MyEntity()
		{

		}

		override public function update():void
		{
			if (Input.check(Key.SPACE))
			{
				// The spacebar is being held down.
			}
			
			if (Input.check(Key.RIGHT))
			{
				// The right-arrow key is being held down.
			}
			
			if (Input.check(Key.G))
			{
				// The letter G is being held down.
			}
		}
	}
}
{% endhighlight %}

So for example, if we wanted to take our graphical Entity from the [FlashPunk Basics][basics] tutorial, and make it move with the arrow keys, we could do something like this:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;
	import net.flashpunk.utils.Input;
	import net.flashpunk.utils.Key;

	public class MyEntity extends Entity
	{
		public function MyEntity()
		{
			// ...
		}

		override public function update():void
		{
			if (Input.check(Key.LEFT)) { x -= 5; }
			if (Input.check(Key.RIGHT)) { x += 5; }
			if (Input.check(Key.UP)) { y -= 5; }
			if (Input.check(Key.DOWN)) { y += 5; }
		}
	}
}
{% endhighlight %}

Input's [check()][input-check] function will return true if the specified key is held down on the corresponding frame of that update, otherwise it will return false.


<h2 id="specific-key-states">Specific Key States</h2>

Sometimes you want to check the state of a key more specifically, though, such as whether it was just pressed, or perhaps if it was just released. FlashPunk provides simple functions for this purpose as well, which are used similarly like this:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;
	import net.flashpunk.utils.Input;
	import net.flashpunk.utils.Key;

	public class MyEntity extends Entity
	{ 

		public function MyEntity()
		{

		}

		override public function update():void
		{
			if (Input.pressed(Key.SPACE))
			{
				// The spacebar was just pressed THIS frame.
			}

			if (Input.released(Key.SPACE))
			{
				// The spacebar was just released THIS frame.
			}
		}
	}
}
{% endhighlight %}

The [pressed()][input-pressed] function will return true if the key was pressed down on that frame, and the [released()][input-released] will return true if it was released. Every frame the Input states are updated, so when a key is pressed down, it will be in a "pressed" state for one frame, and then in a "held" state the next frame and no longer "pressed". So you can call these functions in your Entity updates to manage your game objects' input.

<h2 id="defining-controls">Defining Controls</h2>

Sometimes it's nice to have certain controls mapped to several different keys; since it's a pain to have to repeat long statements checking for the state of each key, FlashPunk allows you to define and check controls by name as well. For example, I'll have our Entity define two different controls, "Shoot" and "Jump", and then assign several keys to each:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;
	import net.flashpunk.utils.Input;
	import net.flashpunk.utils.Key;

	public class MyEntity extends Entity
	{
		public function MyEntity()
		{
			Input.define("Jump", Key.Z, Key.UP);
			Input.define("Shoot", Key.SPACE, Key.X, Key.C);
		}

		override public function update():void
		{
			if (Input.pressed("Jump"))
			{
				// One (or several) of the "Jump" keys was pressed this frame.
			}
			if (Input.check("Shoot"))
			{
				// One (or several) of the "Shoot" keys is being held down.
			}
		}
	}
}
{% endhighlight %}

Input's check(), pressed(), and released() functions all take a Key value or control name as parameters, so you can check for controls just the same as you check for individual keys.

<h2 id="mouse-states">Mouse states</h2>

Similar to how you check for keys, you can check the state of the mouse using several properties provided by Input, like so:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;
	import net.flashpunk.utils.Input;

	public class MyEntity extends Entity
	{
		public function MyEntity()
		{

		}

		override public function update():void
		{
			if (Input.mouseDown)
			{
				// The mouse button is held down.
			}
			if (Input.mousePressed)
			{
				// The mouse button was just pressed this frame.
			}
			if (Input.mouseReleased)
			{
				// The mouse button was just released this frame.
			}
		}
	}
}
{% endhighlight %}

And if you want to know the current coordinates of the mouse, you can do this:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;
	import net.flashpunk.utils.Input;
	import net.flashpunk.FP;

	public class MyEntity extends Entity
	{
		public function MyEntity()
		{

		}

		override public function update():void
		{
			// Assigns the Entity's position to that of the mouse (relative to the Camera).
			x = Input.mouseX;
			y = Input.mouseY;
			// Assigns the Entity's position to that of the mouse (relative to the World).
			x = FP.world.mouseX;
			y = FP.world.mouseY;
		}
	}
}
{% endhighlight %}

[basics]: {% post_url 2001-01-01-flashpunk-basics %} "FlashPunk Basics"
[input]: http://useflashpunk.net/docs/net/flashpunk/utils/Input.html "net.flashpunk.utils.Input"
[key]: http://useflashpunk.net/docs/net/flashpunk/utils/Key.html "net.flashpunk.utils.Key"
[input-check]: http://useflashpunk.net/docs/net/flashpunk/utils/Input.html#check() "net.flashpunk.utils.Input.check()"
[input-pressed]: http://useflashpunk.net/docs/net/flashpunk/utils/Input.html#pressed() "net.flashpunk.utils.Input.pressed()"
[input-released]: http://localhost:4000/docs/net/flashpunk/utils/Input.html#released() "net.flashpunk.utils.Input.released()"
