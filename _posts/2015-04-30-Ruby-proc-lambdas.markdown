---
layout: post
title:  "Ruby Procs and Lambdas"
date:   2015-04-30 11:30:21
---

#Ruby - Proc and Lambdas

Procs are blocks of code that can be associated with a variable, thus;

    >> times3 = Proc.new { |value| value * 3 }
    => #<Proc:0x007fd5811500f0@(irb):1>
    >> times3.call(3)
    => 9
    >> times3.call(5)
    => 15
    >> times3.call(2)
    => 6

Similar behavior could be accomplished with Lambdas.

    >> times5 = lambda { |value| value * 5 }
    => #<Proc:0x007fbc6a171708@(irb):1 (lambda)>
    >> times5.call(3)
    => 15
    >> times5.call(2)
    => 10
    >> times5.call(4)
    => 20
    

*TODO: Give more examples and a better explanation.*
