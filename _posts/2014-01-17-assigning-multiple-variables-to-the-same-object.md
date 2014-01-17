---
layout: post
title: Assigning Multiple Variables to the Same Object
---
So, the blog has been a bit quiet. But this doesn't mean that I haven't been getting some decent studying done.

As I mentioned in my [first post](http://gregelizondo.github.io/2013/12/18/why-im-learning-ruby.html), this isn't the first time I've studied Ruby (or programming in general) so I have a pretty good grasp on the basics. Nevertheless, I thought it'd be best to start from the beginning. Even if that meant re-covering a lot of material. My theory is that by sticking with [one source](http://pragmaticstudio.com/ruby/) at a time, I'll pick up on things that I might have missed by jumping from teacher-to-teacher.

For example, in one recent video, I actually picked up some new knowledge on variable assignment.

##Assigning Multiple Variables to One Object

Let's pretend we're creating a video game. Each character is identified by a certain color. Our first character is blue:

{% highlight ruby %}
 > warrior_color = "blue"
 => "blue"
{% endhighlight %}

But what happens when we assign another character to our first variable:

{% highlight ruby %}
 > archer_color = warrior_color
 => "blue"
{% endhighlight %}

So right now, both of our characters have a color set to the string `"blue"`. If we call the `object_id` method on both of these variables, we'll see that they are in fact both assigned to the same object.

{% highlight ruby %}
 > warrior_color.object_id
 => 2168461020 
 > archer_color.object_id
 => 2168461020
{% endhighlight %}

What happens if we start altering the object?

{% highlight ruby %}
 > # let's swap out the first letter of the first variable
 > warrior_color[0] = "g"
 => "g" 
 > warrior_color
 => "glue" 
 > archer_color
 => "glue"
 > warrior_color.object_id
 => 2168461020 
 > archer_color.object_id
 => 2168461020
 > # the actual object changed but both variables are still assigned to it
{% endhighlight %}

So far, all of this is pretty clear. But this this bit coming up is where I was a little surprised.

{% highlight ruby %}
 > warrior_color = "red"
 => "red" 
 > warrior_color.object_id
 => 2168469920 
 > # new object, new object_id. did the archer_color variable change?
 > archer_color
 => "glue"
 > archer_color.object_id
 => 2168461020
{% endhighlight %}

What took me by surprise was that when I re-assigned the `warrior_color` variable to `"red"`, I expected the `archer_color` variable to change as well (just as it had when I swapped out the "b" for a "g"). Of course, this didn't happen. When I assigned the `warrior_color` to a whole new variable, a brand new object was created. This did not affect the other variable, `archer_color`, which still maintained the same `object_id`