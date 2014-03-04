---
layout: post
title: Getting Started with RSpec and Unit Testing
---
Like many people, when I first encountered the concept of testing, I was hesitant. It seemed like more work for little in return. Needless to say, my opinion on testing has shifted to the other end of the spectrum. There's plenty of great articles on why testing is not only important, but critical for many applications, so I won't go into the *why* of testing.

Instead, I'll give a brief introduction to writing and running unit tests with [RSpec](http://rspec.info/). There's books written on the subject of testing with RSpec ([literally](http://pragprog.com/book/achbd/the-rspec-book)) so it's important to understand that this is only a brief introduction.

##What we're testing

To get started with RSpec, we first need to install it, which is simple enough:

{% highlight ruby %}
> gem install rspec
{% endhighlight %}

This will install RSpec and few other dependencies.

Of course, now we need something to test. Why don't we use a version of our Character class we've looked at in previous posts:

{% highlight ruby %}
# character.rb
class Character

	def initialize(name, health=100)
		@name = name.capitalize
		@health = health
	end
	
	def name
	  name = @name
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

So, we have a simple character Class. It allows us to create a new character, display his name, increase his health, decrease his health, and display his info. To make sure our Class is functioning properly, we can write a series of tests.

##Writing unit tests

Since the Class and `.rb` file we're working is called `character`, RSpec convention says we should name our test file, `character_spec.rb`. RSpec has its own DSL but it's all actually highly readable Ruby code. Let's get started by writing our first test!

{% highlight ruby %}
require_relative 'character'

describe Character do
  
  it "has a capitalized name" do
    character = Character.new("greg")
    
    character.name.should == "Greg"
  end
  
end
{% endhighlight %}

In this test, a few different things are going on. First, we're using `require_relative` in order to let RSpec know which file we're testing. Second, we're starting a block with `describe Character do`, which is where we'll put all of our unit tests for the character Class. Third, we write our first test to make sure a new character's name is capitalized.

We can then hop back over to the terminal and run the test with, `rspec character_spec.rb --color`. The `--color` flag at the end will add some nice color highlighting to our test. This is what you would see after running this test:

<img src="{{ site.url }}/assets/name_unit_test.png" />

Pretty cool, right? But, how are we sure that our test is actually working properly? In Test-Driven-Development, it's a common practice to write a failing test first, describing the outcome that you would like to see happen, followed by writing the appropriate code to match. Let's take a similar approach and see if we can get this code to fail.

Going back to our spec, let's alter the expectation.

{% highlight ruby %}
  it "has a capitalized name" do
    character = Character.new("greg")
    
    character.name.should == "greg" # this should fail
  end
{% endhighlight %}

Now when we run the test, this is what we see:

<img src="{{ site.url }}/assets/failing_unit_test.png" />

Oh no! We've lost our beautiful green. The good news is we now know our test is working properly. We can go ahead and revert that last change to our test and write a couple more tests. Let's check to make sure our other methods are working properly.

{% highlight ruby %}
require_relative 'character'

describe Character do
  
  it "has a capitalized name" do
    character = Character.new("greg")
    
    character.name.should == "Greg"
  end
  
  it "can power_up" do
    character = Character.new("greg")
    
    character.power_up.should == 110
  end
  
  it "can power_down" do
    character = Character.new("greg")
    
    character.power_down.should == 90
  end
  
  it "displays full character info" do
    character = Character.new("greg")
    
    character.character_info.should == "Greg has a health of 100"
  end
  
end
{% endhighlight %}

Now our test suite will check on multiple aspects of our character Class. Let's cross our fingers and run `rspec character_spec.rb --color` again. This is what we see:

<img src="{{ site.url }}/assets/four_unit_tests.png" />

Success! It looks like all of our methods are working properly... there's only one problem left. If you take another look at our `character_spec.rb` file, you'll notice a lot of duplication. We're actually creating a `character` variable for each and every test. This isn't very DRY code and it makes it a little more difficult to read.

Here's what our test would look like after we DRY it up.

{% highlight ruby %}
require_relative 'character'

describe Character do
  
  before do
    @character = Character.new("greg")
  end
  
  it "has a capitalized name" do
    @character.name.should == "Greg"
  end
  
  it "can power_up" do
    @character.power_up.should == 110
  end
  
  it "can power_down" do
    @character.power_down.should == 90
  end
  
  it "displays full character info" do 
    @character.character_info.should == "Greg has a health of 100"
  end
  
end
{% endhighlight %}

There's a couple things to note here. The first is that `before do` block at the top. This piece of code will run before each test, so we no longer have to create a new variable every single time we want to add a test. The second thing is we're now using an instance variable of `@character` so that it's accessible for each test. If we were just using a local variable (such as `character`), this wouldn't be the case.

If we run our test again we'll once again see that all tests are passing.

##A great warning system

The great thing about testing is that it gives you the freedom to hack on other parts of your application with a sense of confidence. Without proper testing, you could inadvertently "break" a different part of your application without realizing your mistake. By the time you notice things aren't working right, it may be too late. A proper test suite will alert you to the problem sooner and let you know which specific parts of your app aren't working.

Hope this helps demystify some of the confusion around testing. Remember, this was just a brief intro and there's plenty of great reading about the TDD methodology. [Find me on Twitter](https://twitter.com/gregelizondo) if you have any questions. Thanks!


