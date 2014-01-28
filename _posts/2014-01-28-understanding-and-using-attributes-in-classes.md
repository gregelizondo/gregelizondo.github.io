---
layout: post
title: Understanding and Using Attributes in Classes
---
A little while ago when I was first trying to learn Rails, I came across a method that looked a little foreign to me. It looked something like this:

{% highlight ruby %}
def title=(new_title)
	@title = new_title
end
{% endhighlight %}

At the time, I thought to myself, "That's and odd way to define a method. I wonder what that equals sign is for." For the first time, I decided to give the [Rails Hotline](http://rails.pockethotline.com/) a call.

While I am sure the friendly operator on the other side of the line was doing his best to explain the concept to me, I just couldn't understand it. After a couple minutes, I eventually got a little embarrassed to be asking the same question over-and-over again so I just said, "ah, I get it now, thanks!"

Fast forward to present day, all of my confusion about Attributes (`attr_reader`, `attr_writer`, and `attr_accessor`) has vanished.

##Object Privacy

In Ruby, the default state of an object is private. Generally speaking, this is a good thing. Let's take a look at this code example to see what I'm talking about:

{% highlight ruby %}
class Character
	def initialize(name, health=100)
		@name = name
		@health = health
	end
end
{% endhighlight %}

Simple enough. Let's create a new character.

{% highlight ruby %}
> character1 = Character.new("Ernie", 95)
=> #<Character:0x000001011adba8 @name="Ernie", @health=95>
{% endhighlight %}

Cool, now if I want to see my character's name, I can just use the `name` method, right?

{% highlight ruby %}
> character1.name
NoMethodError: undefined method `name' for #<Character:0x000001011adba8 @name="Ernie", @health=95>
	from (irb):8
{% endhighlight %}

Oh no, what happened? Well, according to the error it looks like there is no method called `name` (yet). As I mentioned earlier, the default state of an object in Ruby is private. So if we want to be able to read the state of an object, we're going to need to create the method inside the class.

{% highlight ruby %}
class Character
	def initialize(name, health=100)
		@name = name
		@health = health
	end
	
	# add a name method so we can display our character's name
	def name
		@name
	end
end
{% endhighlight %}

Now when we call the `name` method in irb...

{% highlight ruby %}
> character1.name
=> "Ernie"
{% endhighlight %}

Sweet, it works! No more NoMethodError. We've overridden the default privacy state of our object.

##attr_reader, attr_writer, and attr_accessor

The above process definitely works. But, the code is a little cumbersome. It also seems like we shouldn't have to explicitly write something out like this if it's going to come up so often. Luckily, Ruby makes this process even easier.

If we're just trying to provide our object with the ability to be read, the `attr_reader` attribute will do the trick. It looks like this:

{% highlight ruby %}
class Character
	# make the name state readable
	attr_reader :name
	
	def initialize(name, health=100)
		@name = name
		@health = health
	end
	
	# this method is no longer necessary thanks to attr_accessor
	#def name
	#	@name
	#end
end
{% endhighlight %}

With this change, we can still call the `name` method on our variable `character1` just like before (By the way, the `:name` after `attr_reader` is a symbol, which I have a decent grasp on but have yet to fully understand. I will write more about it in the future.). However, `attr_reader` only makes it so we can **read** our object's state, not **alter** it. If we want to be able to change a state, we'll need a new method (this is where the method with a "=" at the end of it comes in).

{% highlight ruby %}
class Character
	# make the name state readable
	attr_reader :name
	
	def initialize(name, health=100)
		@name = name
		@health = health
	end
	
	# this method allows us to change an object's state
	def name=(new_name)
		@name = new_name
	end
end
{% endhighlight %}

Let's see how it works in `irb`.

{% highlight ruby %}
> character1
=> #<Character:0x000001011adba8 @name="Ernie", @health=95>

# now let's change our character's name

> character1.name = "Freddy"
=> "Freddy" 
> character1
=> #<Character:0x000001011adba8 @name="Freddy", @health=95>
{% endhighlight %}

By now, you may be able to guess that there's a better way to do this. Rather than writing a whole new method called `name=`, we can use the `attr_writer` attribute:

{% highlight ruby %}
class Character
	# make the name state readable
	attr_reader :name
	# make the name state writable
	attr_writer :name
	
	def initialize(name, health=100)
		@name = name
		@health = health
	end
end
{% endhighlight %}

Great, now our `name` state for the `Charater` class is both readable and writable. We can improve our code one step further by using the `attr_accessor` attribute which gives both reading and writing permissions.

{% highlight ruby %}
class Character
	# make the name state readable and writable
	attr_accessor :name
	
	def initialize(name, health=100)
		@name = name
		@health = health
	end
end
{% endhighlight %}

Now we have both the ability to read and write the `name` state of our variable.:

{% highlight ruby %}
> character1.name
=> "Freddy" 
> character1.name = "Jimbob"
=> "Jimbob"
{% endhighlight %}

I hope this helps somebody else who has been struggling with attributes in Ruby. Hopefully it will save you an embarrassing phone call!























