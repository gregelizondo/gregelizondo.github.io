---
layout: post
title: Learning How to Refactor Your Ruby Code with Codewars
---

If you've ever poked around online for in-browser coding playgrounds, you know that there's no shortage of fun ways to practice your coding skills. But if you're like me, you've probably also found a lot of them to be somewhat limiting.

For example, I absolutely loved running through [Ruby Koans](http://rubykoans.com/) and [Ruby Monk](https://rubymonk.com/), but both of them left me wanting more (by the way, if you're new to Ruby, be sure to check out both of those sites). I certainly learned a lot, but it never felt like I was solving "real" coding problems. I wanted something a little more "real world."

Enter: [Codewars](http://www.codewars.com/r/BWz9Tw).

##Solving and learning

For those of you that are unfamiliar with Codewars, let me tell you a little bit about how it works.

It's a gamified take on programming challenges. Users are able to choose their language(s) of study and then take on individual coding challenges called Katas. As harder and harder Katas are solved, the user can level up his/her rank (referred to as Kyu).

What makes Codewars so enjoyable is the community aspect. Once you successfully solve a Kata, you're shown the solutions of other Codewars members. Which—for a newbie programmer—can be a humbling and educational experience.

When I first started practicing on Codewars, this was a common scenario: I'd rack my brain for 20-30 minutes on a difficult problem. Finally, I'd finish the exercise and submit my final answer. My joy would quickly turn to disappointment as I reviewed the top solutions submitted by other members. What took me 8-10 lines of code, others were doing in 1!

But this is why Codewars is such an awesome tool for newer programmers. After seeing how other (better) programmers solved the challenges, some of their methods started working their way into my thought process.

##Learning how to refactor

Here's an example Kata:

{% highlight text %}
Complete the solution so that it takes the hash and 
generates a human readable string from its key/value pairs.

The format should be "KEY = VALUE".
Each key/value pair should be separated by a comma 
except for the last pair.

Example:
solution({a: 1, b: '2'}) # should return "a = 1,b = 2"
{% endhighlight %}

And here's what my original solution to this exercise was:

{% highlight ruby %}
def solution(pairs)
  string = ""
  pairs.each do |k,v|
    string << "#{k} = #{v},"
  end
  string.chomp(',')
end
{% endhighlight %}

Well, it works. But it certainly isn't the prettiest code. Once I submit my final answer, I get to see how other Codewars members solved the problem and can even sort by *Best Practices*. Here's what the top solution looks like:

{% highlight ruby %}
def solution(pairs)
  pairs.map{|k,v| "#{k} = #{v}"}.join(',')
end
{% endhighlight %}

Ah, much more elegant.

Usually the *Best Practice* solution makes sense to me. I study them and try and commit them to memory. Occasionally, I'll need to dig into [ruby-doc.org](http://ruby-doc.org/) or do a little Googling to make sense of the more complicated solutions. Regardless, it's a great way of learning how more advanced programmers think.

If you haven't already, be sure to give [Codewars](http://www.codewars.com/r/BWz9Tw) a try.