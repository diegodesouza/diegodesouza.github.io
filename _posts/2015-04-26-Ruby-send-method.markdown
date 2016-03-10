---
layout: post
title:  "Ruby Methods - Send"
date:   2015-04-26 11:30:21
categories: posts
---


# Ruby Methods and Objects - Send

I want to get more acquainted with the Ruby language. I want to be able to explore its methods in more depth and be able to implement them better in my work. 
*This is meant for me primarily, although this might be helpful to some people, I encourage you to read the docs, and/or other articles/blogs to explore and learn more.*

Today I chose the object `send`.

According to the [docs](http://ruby-doc.org/core-2.2.2/Object.html#method-i-send). Invokes the method identified by *symbol*, passing it any arguments specified.
You can use `__send__` if the name *send* clashes with an existing method in obj. When the method is identified by a string,
the string is converted to a symbol.

    class Klass
      def hello(*args)
        "Hello " + args.join(' ')
      end
    end
    k = Klass.new
    k.send :hello, "gentle", "readers"   #=> "Hello gentle readers"
     
We can call any method on an object by using the *send* method. *Send* takes, as its first argument, the name of the method that you want to call.
This name can either be a symbol or a string. Like so.

    send(symbol [, args...]) → obj 
    __send__(symbol [, args...]) → obj
    send(string [, args...]) → obj
    __send__(string [, args...]) → obj

With this, everytime we call a method on an object, we're *sending* that object a *message*. Where this *message* is the name of the method we called. 
For example:

    1.send('to_s')
    => '1'

Above we're calling the method `to_s` on the number `1`. In other words we're sending the *message* `to_s` to the instance of the class *FixNum*.

**Interesting enough**, we can use *send* on private methods as well.

Consider this case.


    ?> class Foo
    >>   private
    >>   def say_hello(name)
    >>     puts "Hello, #{name}."
    >>   end
    >> end
    => nil
    >>
    ?> example = Foo.new
    => #<Foo:0x007fa5da884398>
    >> example.say_hello('Diego')
    NoMethodError: private method `say_hello' called for #<Foo:0x007fa5da884398>
    	from (irb):108
    	from /Users/diegodesouza/.rubies/ruby-2.0.0-p643/bin/irb:12:in `<main>'
    >>
    >> example.say_hello
    NoMethodError: private method `say_hello' called for #<Foo:0x007fa5da884398>
	    from (irb):109
	    from /Users/diegodesouza/.rubies/ruby-2.0.0-p643/bin/irb:12:in `<main>'
    >> example.send(:say_hello, 'Diego')
    Hello, Diego.
    => nil
    >>

Oddly enough we do have access to methods otherwise said private. 


