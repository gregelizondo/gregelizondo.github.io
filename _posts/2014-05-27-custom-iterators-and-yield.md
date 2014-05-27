---
layout: post
title: Custom Iterators and yield
---
Iterators are a crucial component of the programming world. Ruby make iterators easy to use and read.

Iterating over a simple array may look something like this:

{% highlight ruby %}
> plain_numbers = [2, 3, 4]
=> squared_values = []

> plain_numbers.each do |n|
>   squared_values << n**2
> end

> squared_values
=> [4, 9, 16]
{% endhighlight %}

This snippet of code should be simple enough to understand. First, we created an array called `plain_numbers`. We also created an empty array named `squared_values`.

The next section of code is called a block in Ruby. The `each` method signifies that we'll be going through each element in the object (in this case it's the `plain_numbers` array) and interacting with it. The element gets passed in to the block of code as the `n` variable. We take this `n` variable, square it, and then add it to the `squared_values` array. Simple enough.

Ruby comes with a bunch of iterators built-in, like `each`, `times`, `upto, and  `each_index`. These iterators are simple but powerful and you'll see them being used all over Ruby code.

##Rolling your own iterator

Sometimes a situation might arise when the built-in Ruby iterators just won't cut it. What then? Luckily, Ruby makes it pretty easy to write your own custom iterator.

Here is a simple case of writing our own custom iterator:

{% highlight ruby %}
def simple_block
	puts "This is in the method"
	yield
	puts "This is still in the method"
	yield
end

simple_block {puts "This is in the block"}
{% endhighlight %}

Would result in:
{% highlight text %}
This is in the method
This is in the block
This is still in the method
This is in the block
{% endhighlight %}

As you can see, the block is being invoked by calling the method of the same name, `simple_block`. When we first call `simple_block`, the method starts and `puts "This is in the method"` is called. `yield` then hands off responsibility to the block (which is denoted by the curly brackets). Once this block is done running, we're back in the original method again. This process happens one more time when `yield` is called yet again.

When I first encountered the `yield` statement, I found it incredibly confusing. I still can't claim mastery over it, but simple examples like the one above helped me to better understand it.

Here's another simple example of a custom iterator in use. Let's pretend we're building a game that needs to check whether or not a character's name is valid. For whatever reason, we only want character names that are greater than 3 characters.

{% highlight ruby %}
character_names = ['Jon', 'Ricky', 'Andrew']

def valid_character_checker(names)
  names.each do |name|
    puts "Is #{name} a valid charcter?"
    yield(name)
  end
end

valid_character_checker(character_names) do |name|
  if name.length > 3
    puts "This is a valid character name!"
  else
    puts "This is an invalid character name :("
  end
end
{% endhighlight %}

Once again, this example is pretty simple if you work through it step-by-step. It's also pretty verbose and probably not the best example of what *real* code would look like in the wild.

Do you have a better example of how `yield` and custom iterators should be used in Ruby? Comment below and let me know.