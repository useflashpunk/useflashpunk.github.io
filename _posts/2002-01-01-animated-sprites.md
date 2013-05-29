---
layout: tutorial
title: Animated Sprites
category: intermediate
---

In [FlashPunk Basics][1], we covered [embedding graphics][2] and assigning them to entities Entity as [Image][3] objects. Since the Image class is only for single-frame images, this tutorial will cover how to create an animated graphic using FlashPunk's [Spritemap][4] class.

 - Create a Spritemap
 - Create Animations
 - Playing Animations


Create a Spritemap
--

The first thing we need to do is create a [Spritemap][5] object for our Entity, similar to how we created an Image before. For this example, I'll be animating this spritesheet: 

> <img src="/uploads/default/15/c26c6646668e1482.png" width="288" height="64">
> 
> *2010 © Tyriq Plummer*

The spritesheet's total size is 288×64 pixels, but each individual frame is 48×32 pixels, which is what we need to know to create it into a [Spritemap][6] object. So in this example, we will create a Player class and embed the PNG first: 

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;

	public class Player extends Entity
	{
		[Embed(source = 'assets/swordguy.png')]
		private const SWORDGUY:Class;

		public function Player()
		{

		}
	}
}
{% endhighlight %}

Next, we'll create the Spritemap object and assign it to a variable: 

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;
	import net.flashpunk.graphics.Spritemap;

	public class Player extends Entity
	{
		[Embed(source = 'assets/swordguy.png')]
		private const SWORDGUY:Class;

		public var sprSwordguy:Spritemap = new Spritemap(SWORDGUY, 48, 32);

		public function Player()
		{

		}
	}
}
{% endhighlight %}

So as you see here, creating a Spritemap is similar to creating an Image. You create the object and pass the source file (in this case, SWORDGUY) into it. But since a Spritemap does not display a single image, but rather individual frames of a spritesheet, it needs to know the frame size. So I pass in the width and height as well (48 and 32).

Creating animations
--

Now, we want to assign specific animations to our spritemap. So our **sprSwordguy** spritesheet has two animations in this spritesheet, a standing animation (top), and a running animation (bottom), both which are 6 frames long. We assign animations to a spritesheet by using its [add(][7]) function, like this: 

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;
	import net.flashpunk.graphics.Spritemap;

	public class Player extends Entity
	{
		[Embed(source = 'assets/swordguy.png')]
		private const SWORDGUY:Class;

		public var sprSwordguy:Spritemap = new Spritemap(SWORDGUY, 48, 32);

		public function Player()
		{
			sprSwordguy.add("stand", [0, 1, 2, 3, 4, 5], 20, true);
			sprSwordguy.add("run", [6, 7, 8, 9, 10, 11], 20, true);
		}
	}
}
{% endhighlight %}

Here, I create two animations, "stand" and "run". The add() function takes 4 parameters, which are as follows:

 - Name (a string)
 - Frames (an array frames to play)
 - Speed (frames per second)
 - Looping (true or false)

You name the animation so you can access it later, when you want to play or switch animations. Then, you assign a set of frames to it. The top-left frame in a spritesheet is frame 0, and they count up left-to-right and then top-to-bottom. 

> <img src="/uploads/default/16/0ab577aad6215a76.png" width="288"
> height="64">
> 
> *An example of the frame index layout.*

The third value is the animation speed, in frames per second, at which you want it to animate. I chose 20 for these two particular animations. And finally, the last parameter is whether you want the animation to loop or not. Non-looping animations will stop on the last frame and stay there until you start them again or change the animation. Looping animations will keep cycling through the selected frames infinitely until you stop or change the animation. 

Finally, don't forget to assign the Spritemap to the Entity, otherwise it won't be displayed: 

{% highlight actionscript %}
package
{
	import net.flashpunk.Entity;
	import net.flashpunk.graphics.Spritemap;

	public class Player extends Entity
	{
		[Embed(source = 'assets/swordguy.png')]
		private const SWORDGUY:Class;

		public var sprSwordguy:Spritemap = new Spritemap(SWORDGUY, 48, 32);

		public function Player()
		{
			sprSwordguy.add("stand", [0, 1, 2, 3, 4, 5], 20, true);
			sprSwordguy.add("run", [6, 7, 8, 9, 10, 11], 20, true);
			graphic = sprSwordguy;
		}
	}
}
{% endhighlight %}

Playing animations

Once you've created your desired animations, you can tell them which animation to play using the Spritemap's [play()][8] function, like this: 

{% highlight actionscript %}
sprSwordguy.play("stand");
{% endhighlight %}

So here, I call the play() function and tell it to play my pre-assigned "stand" animation. The animation will then start at the first frame, animate through the assigned frames, and loop continuously if desired. You can call play() at any time to switch which animation your Entity is playing. 
TIP

> If you tell your Spritemap to play an animation that it is already
> currently playing, nothing will happen. But if you want it to reset
> the animation and start back at the beginning, you can invoke the
> reset parameter, like this:

{% highlight actionscript %}
sprSwordguy.play("stand", true);
{% endhighlight %}

  [1]: {% post_url 2001-01-01-flashpunk-basics %}
  [2]: {% post_url 2001-01-01-flashpunk-basics %}
  [3]: http://useflashpunk.net/docs/net/flashpunk/graphics/Image.html
  [4]: http://useflashpunk.net/docs/net/flashpunk/graphics/Spritemap.html
  [5]: http://useflashpunk.net/docs/net/flashpunk/graphics/Spritemap.html
  [6]: http://useflashpunk.net/docs/net/flashpunk/graphics/Spritemap.html
  [7]: http://useflashpunk.net/docs/net/flashpunk/graphics/Spritemap.html#add%28%29
  [8]: http://useflashpunk.net/docs/net/flashpunk/graphics/Spritemap.html#play%28%29

*Original tutorial by Chevy Ray Johnston*