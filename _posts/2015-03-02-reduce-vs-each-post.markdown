---
layout: post
title:  ".reduce vs. .each in Ruby "
date:   2015-03-02 11:30:21
categories: jekyll update
---

# Simplicity

When iterating through an array, if you don’t want a return value from the operations,
use ```each```; and if you’re looking for a return value, use the iterator method that
delivers the type of value you want returned. So if you want to take a collection and
return a subset of that collection based on some criteria, use ```select```.
If you want to return a transformed version of each element, use ```map```. And
if you want to return any value whatsoever, or a value that doesn’t match another
iterator method, use ```reduce```.

```reduce``` takes in an array and reduces it to a single value. It does this by
iterating through a list, keeping and transforming a running total along the way.
Each iteration starts with the return value of the previous iteration and does
something with it.


```(5..10).reduce(:+)``` #=> 45

or same as

```(5..10).reduce {|a, b| a + b}``` #=> 45

also same as

```sum = 0
(5..10).each do |value|
  sum += value
end
sum

#=> 45```
