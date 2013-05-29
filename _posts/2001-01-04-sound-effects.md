---
layout: tutorial
title: Sound Effects
category: basic
---

In this tutorial, I will teach you how to embed and play sound effects in your game, as well as how to alter their volume and panning 

 - Embedding Sounds
 - Playing Sounds
 - Volume & Panning

Embedding Sounds
--

Embedding sound effects is very similar to how you [embed graphics][1] in your game. If we have our **Player** Entity, we can embed a sound called "shoot.mp3″ contained in our assets folder like so:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;

	public class Player extends Entity
	{
		[Embed(source = 'assets/shoot.mp3')] private const SHOOT:Class;

		public function Player()
		{

		}
	}
}

{% endhighlight %}
> Remember, if you're using FlashDevelop, you can right-click on the audio file in your
> project and choose "Generate embed code" to automatically insert an
> asset metatag at the cursor.

This embeds the sound effects and assigns it to a variable called SHOOT, so we can access it.

Playing sounds
--
Sounds in FlashPunk are created and played using [Sfx][2] objects. Here, I'll create a Sfx object from our embedded sound and assign it to a variable in our Player:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;
	import net.flashpunk.Sfx;

	public class Player extends Entity
	{
		[Embed(source = 'assets/shoot.mp3')]
		private const SHOOT:Class;

		public var shoot:Sfx = new Sfx(SHOOT);

		public function Player()
		{

		}
	}
}
{% endhighlight %}

So now, our Player can access this sound effect at any time to [play()][3] the sound, like this:

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;
	import net.flashpunk.Sfx;
	import net.flashpunk.utils.Input;
	import net.flashpunk.utils.Key;

	public class Player extends Entity
	{
		[Embed(source = 'assets/shoot.mp3')]
		private const SHOOT:Class;

		public var shoot:Sfx = new Sfx(SHOOT);

		public function Player()
		{

		}

		override public function update():void
		{
			if (Input.pressed(Key.SPACE))
			{
				// Play the sound when the spacebar is pressed.
				shoot.play();
			}
		}
	}
}
{% endhighlight %}

That will play the sound once. If you want the sound effect to loop (for example, background music or perhaps a running motor), you can use the [loop()][4] function instead, like this:

{% highlight actionscript %}
shoot.loop();
{% endhighlight %}

And stopping a sound effect that is playing simply requires the [stop()][5] function:

{% highlight actionscript %}
shoot.stop();
{% endhighlight %}

Volume and panning
--


FlashPunk allows you to change the global volume and panning factor of sound effects by assigning the [FP.volume][6] and [FP.pan][7] properties, like this:

{% highlight actionscript %}
// Sets the volume to 50%.
FP.volume = 0.5;
// Pans all sounds to the left speaker.
FP.pan = -1;
{% endhighlight %}

But sometimes you want to change the volume or panning factor of an individually playing sound effect, which is also possible using the Sfx class. If you have a Sfx object, you can assign a volume and panning factor to the sound when you call its [play()][8] or [loop()][9] function, by passing in the desired parameters:

{% highlight actionscript %}
// Play the sound with 50% volume and no panning.
mySfx.play(0.5); // Play the sound with 100% volume, panned to the right speaker.
mySfx.play(1, 1);
{% endhighlight %}

And if you want to alter the volume or panning factor of a sound effect during playback, you can just assign the Sfx object's [volume][10] and [pan][11] properties like so:

{% highlight actionscript %}
// Set the sound's volume to 25%.
mySfx.volume = 0.25;
// Pan the sound to the left speaker by 50%.
mySfx.pan = -0.5;
{% endhighlight %}

  [1]: {% post_url 2001-01-01-flashpunk-basics %}
  [2]: http://useflashpunk.net/docs/net/flashpunk/Sfx.html
  [3]: http://useflashpunk.net/docs/net/flashpunk/Sfx.html#play%28%29
  [4]: http://useflashpunk.net/docs/net/flashpunk/Sfx.html#loop%28%29
  [5]: http://useflashpunk.net/docs/net/flashpunk/Sfx.html#stop%28%29
  [6]: http://useflashpunk.net/docs/net/flashpunk/FP.html#volume
  [7]: http://useflashpunk.net/docs/net/flashpunk/FP.html#pan
  [8]: http://useflashpunk.net/docs/net/flashpunk/Sfx.html#play%28%29
  [9]: http://useflashpunk.net/docs/net/flashpunk/Sfx.html#play%28%29
  [10]: http://useflashpunk.net/docs/net/flashpunk/Sfx.html#volume
  [11]: http://useflashpunk.net/docs/net/flashpunk/Sfx.html#pan

*Original tutorial by Chevy Ray Johnston*
