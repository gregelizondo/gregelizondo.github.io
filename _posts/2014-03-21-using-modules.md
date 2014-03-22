---
layout: post
title: Using Modules
---

Modules are an important part of the Ruby programming world. They offer multiple benefits such as the ability to:

+ "mix-in" methods across multiple classes
+ DRY out your code
+ bundle logically related objects together

##What code would look like without modules

Take a look at this sample code and notice what's wrong with it:

{% highlight ruby %}
#car.rb
class Car
  def horn
    puts "beep beep!"
  end
end

#truck.rb
class Truck
  def horn
    puts "beep beep!"
  end
end
{% endhighlight %}

A couple things look a little funky here. We have 2 different classes present and they both define a method called `horn`.

First off, this code isn't DRY. We define the same method in 2 different classes that serve the same exact function. Second, the `horn` method isn't exclusive to the `Car` or `Truck` classes. They can make use of this method, but so could a lot of other objects. Who knows, further down the line we may be creating `Van` and `Bus` classes that also need a `horn` method. It wouldn't make sense to re-define it for all of them too.

And this, is where modules come in handy.

##A "bucket" for methods

One way of describing modules that really helped me was thinking of them as "buckets" for methods. They're similar to classes in this way, but unlike classes they can't be instantiated.

So, let's create a new module and `include` it in our 2 classes.

{% highlight ruby %}
#car.rb
class Car
  include AutomobileSounds
end

#truck.rb
class Truck
  include AutomobileSounds
end

#automobile_sounds.rb
module AutomobileSounds
  def horn
    puts "beep beep!"
  end
end
{% endhighlight %}

We can hop into irb and test out the functionality:
{% highlight ruby %}
> load 'automobile_sounds.rb'
=> true
> load 'car.rb'
=> true
> Car.new.horn
=> beep beep!
{% endhighlight %}

Pretty sweet. Our new module can now be accessed by both of our other classes. We've DRY'ed up our code, made it more elegant, and made it easier to use as our application grows.

As I mentioned earlier, unlike classes, modules can't be instantiated so `AutomobileSounds.new` will give us an "undefined method" error. This module is merely a "bucket" for methods.

It's also important to note that using the `include` method gives us access to a module as an instance method. For example, we used `include AutomobileSounds` in the `Truck` class so we can now call `Truck.new.horn`. We cannot, however, call `Truck.horn`, which would be a class method. If we wanted to be able to call a class method, we would have to `extend` the module, rather than `include` it. Make sense?

##Closing

As always, please let me know if anything doesn't make sense or if you notice anything that needs fixing. You can reach me quickly [@gregelizondo](https://twitter.com/gregelizondo). I'm always open to hearing feedback. Thanks!

*If you prefer a video explanation of this concept, be sure to check out [Richard Schneeman's](https://twitter.com/schneems) great video on YouTube: [Using Modules in Ruby](https://www.youtube.com/watch?v=taaIIYj1jFA). This post is basically a written version of his lesson.*