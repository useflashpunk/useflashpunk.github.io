---
layout: tutorial
title: Timing &amp; Motion
category: intermediate
---

If you've already read [FlashPunk Basics][basics] and [Keyboard & Mouse Input][input], you know that you can change an Entity's position by altering its [x][entity-x] and [y][entity-y] properties, which will, in turn, alter the position at which it is drawn to the screen. This tutorial will cover some important things to know about timing and motion while using FlashPunk, related to the timestep chosen when your game starts.

## Contents

<ul class="nav nav-pills nav-stacked">
	<li><a href="#choosing-a-timestep">1. Choosing A Timestep</a></li>
	<li><a href="#basic-timing">2. Basic Timing</a></li>
	<li><a href="#basic-motion">3. Basic Motion</a></li>
</ul>

<h2 id="choosing-a-timestep">Choosing A Timestep</h2>

FlashPunk supports two different types of timesteps:

 - Fixed timestep
 - Variable timestep

When your FlashPunk Engine starts in your Main class, you choose which timestep type you want to use:

{% highlight actionscript %}
package
{
	import net.flashpunk.Engine;

	public class Main extends Engine
	{
		public function Main()
		{
			super(800, 600, 60, false);
		}
	}
}
{% endhighlight %}

The 4th parameter in [Engine's constructor][engine-constructor], "fixed", is whether you want your chosen framerate to be fixed or not. If I choose **true**, I am using a fixed timestep; if I choose **false** (as in the above code), I am using a variable timestep.

How you do motion and timing differs based on which timestep type you are using in your game. The next steps will outline those differences and explain how to accomplish each with both timestep types. But before you start, it is important to note:

 - With a **fixed** timestep, time is in **frames**.
 - With a **variable** timestep, time is in **seconds**.

This is because a fixed timestep tries to run at a constant framerate (for example, 60 frames per second). With a variable timestep it doesn't matter, your game will just loop at whatever speed it can. This is why, while using a variable timestep, your timing & motion will use the [FP.elapsed][fp-elapsed] property to judge how much time has passed since the last frame. Read on.

<h2 id="basic-timing">Basic Timing</h2>

Timing is a huge part of developing games. One very simple thing you will use very often is counters, which is basically variables that increase (or decrease) in value every frame; then, you can have certain events performed when the counter reaches (or exceeds) a certain value.

**Fixed Timestep**

If you are using a fixed timestep, your counters operate in **frames**, so a simple timing system might look like this:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;

	public class Player extends Entity
	{
		public function Player()
		{

		}

		public var time:int = 0;

		override public function update():void
		{
			time ++;
			if (time == 60)
			{
				// Reset the counter after 60 frames has elapsed.
				time = 0;
			}
		}
	}
}
{% endhighlight %}

So basically, the code within those brackets will be performed every 60 frames. If your game's framerate is set to 60 frames per second, this is about once every second.

**Variable Timestep**

When using a variable timestep, your counters operate in **seconds**, so a simple timing system would use [FP.elapsed][fp-elapsed] like this:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;
	import net.flashpunk.FP;

	public class Player extends Entity
	{
		public function Player()
		{

		}

		public var time:Number = 0;

		override public function update():void
		{
			time += FP.elapsed;
			if (time >= 2)
			{
				// Reset the counter after 2 seconds has elapsed.
				time = 0;
			}
		}
	}
}
{% endhighlight %}

So this is a bit more complicated. First, our **time** var must be a [Number][] type, because we're working with decimal values. Second, instead of increasing our counter by 1 (++) every frame, we increase it by [FP.elapsed][fp-elapsed]. FP.elapsed is equal to the amount of seconds passed since the last frame, which means that if we keep adding to **time**, time will exceed 2 once more than 2 seconds has passed. So we check to see if 2 seconds has passed, and then reset the timer.

> If you're creating a looping timer like in the above example, here's
> an even better way to do it:
{% highlight actionscript %}
override public function update():void
{
	time += FP.elapsed;
	if (time >= 2)
	{
		time -= 2;
	}
}
{% endhighlight %}
> Instead of setting **time** to 0, I instead subtract the timer duration
> from it. Since the time passed is usually more than 2 seconds, this
> means that if we set it to 0, that extra time is being discarded and
> ignored. If we instead subtract the total, our timer's next loop will
> start with that extra time added onto it, meaning our timer will cycle
> accurately without losing any time!


<h2 id="basic-motion">Basic Motion</h2>

This will not cover complex movement, just show you the difference between motion fixed and variable timestep motion with a simple moving object.

**Fixed Timestep**

A basic moving object with a fixed timestep might look like this:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;

	public class Player extends Entity
	{
		public function Player()
		{

		}

		// Movement speed (pixels per frame).
		public var speed:Number = 5;

		override public function update():void
		{
			x += speed;
		}
	}
}
{% endhighlight %}

So here I just create a **speed** variable, which is the Player's speed in **pixels per frame**, and then add that value to the Entity's x-position in its update to move it right at the assigned speed. I could subtract that value to move it left, or simply alter the y-position instead if I wanted it to move vertically.

**Variable Timestep**

When using a variable timestep, basic motion works like this:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;
	import net.flashpunk.FP;

	public class Player extends Entity
	{
		public function Player()
		{

		}

		// Movement speed (pixels per second).
		public var speed:Number = 50;

		override public function update():void
		{
			x += speed * FP.elapsed;
		}
	}
}
{% endhighlight %}

This time, our **speed** variable represents our movement speed in **pixels per second**. Then, all we have to do to move our Entity is add the speed to the x-position like before, but this time we multiply it by our handy [FP.elapsed][fp-elapsed] property. Since FP.elapsed is equal to the time elapsed since the previous frame, this means that if 10% of a second passed, you will move by 1/10th of your movement speed. So after 1 entire second passes, you will have moved 1 entire length of your speed (50 pixels).

> While this might seem like extra work for a simple task like movement,
> when using a variable timestep it's a good way to ensure smooth
> motion. Since frame-length can vary during your game loop, fast-moving
> objects would appear to move quite jerky if the player's computer is
> not running Flash Player smoothly, or if your game is using lots of
> CPU and slowing down a bit. Applying this formula to your movement is
> a good way to ensure smooth movement on a wider range of computers and
> games. Plus, many game developers prefer to work with their
> speed/velocity values in pixels per second anyways, since the duration
> of a second is easier to imagine than that of a frame.
> 
> **Warning:**
> Using this method for motion can cause slightly varied results for
> certain types of movement in your game, so it's important to be wary
> of what type of game you are designing before choosing a timestep
> type. If your game is a twitch-platformer that requires lots of
> pixel-perfect jumps and movement, using a variable timestep is
> probably not the best option since jump-height can slightly vary; for
> that, a fixed timestep is probably a better choice, since everything
> is operating at the frame-level. But for a top-down shooter or perhaps
> tower-defense game, this method would work just fine.

[basics]: {% post_url 2001-01-01-flashpunk-basics %}
[input]: {% post_url 2001-01-02-keyboard-and-mouse-input %}
[entity-x]: http://useflashpunk.net/docs/net/flashpunk/Entity.html#x
[entity-y]: http://useflashpunk.net/docs/net/flashpunk/Entity.html#y
[engine-constructor]: http://useflashpunk.net/docs/net/flashpunk/Engine.html#Engine%28%29
[fp-elapsed]: http://useflashpunk.net/docs/net/flashpunk/FP.html#elapsed
[number]: http://www.adobe.com/livedocs/flash/9.0/ActionScriptLangRefV3/Number.html
