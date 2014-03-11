---
layout: post
title: Tell, Don't Ask and Conditionals
---

For newer developers (i.e. me), conditionals are our first opportunity to see real programming logic in action. We get to make a computer do the thinking for us. *If this happens, do this, if not, do this instead.*

Unfortunately, once a newer programmer has a good understanding of `if` and `else` statements, some bad habits can start to occur.

In previous posts, I've referenced our `Character` class, seen here:

{% highlight ruby %}
# character.rb
class Character

	def initialize(name, health=100)
	  @name = name.capitalize
	  @health = health
	end
	
	def health
	  health = @health
	end
	
	def power_up
	  @health += 10
	end
	
	def power_down
	  @health -= 10
	end
	
	def character_info
	  "#{@name} has a health of #{@health}"
	end
	
end
{% endhighlight %}

If we wanted to determine whether or not a `Character` is healthy, we may think about writing some code like this:

{% highlight ruby %}
# character_health.rb
character1 = Character.new("Joe", 80)

if character1.health >= 85
  puts "Healthy"
else
  puts "Unhealthy"
end
{% endhighlight %}

If we ran this piece of code, we would see an output of "Unhealthy" (poor Joe). But really, this little bit of code is violating the "Tell, Don't Ask" principle. We're getting the health of a character and then making a determination based on that information whether or not this character is healthy. We're *asking*, rather than *telling*.

A `Character` should be telling us whether or not its healthy. We shouldn't be asking it. Which means our if/else statement (or something equivalent to it) should live in the `character.rb` file. Also, since we're dealing with a true or false situation (either the character is "healthy" or "unhealthy"), we can be using a boolean.

Going back to our `character.rb` file, we can add this code to our `Character` class.

{% highlight ruby %}
# character.rb
class Character

...

  def healthy?
    @health >= 85
  end

{% endhighlight %}

Now we're telling any instances of the `Character` class, "you are healthy if your health is greater than or equal to 85."

If we still wanted to output a string saying whether or not a character is "healthy" or "unhealthy", we could simply add a new `status` method.

{% highlight ruby %}
# character.rb
class Character

...

  def healthy?
    @health >= 85
  end
  
  def status
    healthy? ? "Healthy" : "Unhealthy"
  end

{% endhighlight %}

There, nice and clean one-line methods! Hope this help some other new Rubyist out there.
