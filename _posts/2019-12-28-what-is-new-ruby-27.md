---
layout: post
title: What is new in Ruby 2.7.0
date: 2019-12-28 19:51:02 -0300
categories: [general, programming-news]
tags: [ruby,rails]
description: what is new in Ruby 2.7.0
---
After some controversial decisions between Matz and the community, Ruby 2.7.0 finally came out right on time to our Christmas be complete. This is the last release before the exciting Ruby 3.0 coming up in December of 2020.

Ruby 2.7.0 also begins to prepare all the Rubyists to the major changes that it is going through on the next year's release by showing warnings, experimental features and so forth. Here I'm going to present you some of the changes that 2.7.0 introduced.

## Numbered Parameters

Now we can access the arguments passed to a block without having to define the block-argument between pipes ||.

```ruby
[1, 2, 3].select { _1 > 2 } #=> [3]
```

As pointed [here](https://bugs.ruby-lang.org/issues/4475), sometimes the array name is so self-explanatory that we don't really need a block variable name to make it more readable. e.g. `Books.all.each {|b| p b.title}`. In this context, numbered parameters come really in hand by saving us some keystrokes without losing readability.

```ruby
Books.all.each { p _1.title }

```

## Pattern Matching

Following the trend of all the major programming languages in adopting functional programming, Ruby is bringing `Pattern Matching` as an experimental feature. [See discussion here](https://bugs.ruby-lang.org/issues/14912)

```ruby
case [10, [20, 30, 40]]
    in [num1,[num2, *num34] ]
        p num1
        p num2
        p num34
end
(irb):1: warning: Pattern matching is experimental, and the behavior may change in future versions of Ruby!
10
20
[30, 40]
```

It also can traverse a hash object and assign its value if a pattern is matched.

```ruby
require "json"

json = <<END
{
  "actor": "Keanu Reeves",
  "age": 55,
  "movies": [{ "title": "The Matrix", "year": 1999 }]
}
END

case JSON.parse(json, symbolize_names: true)
in {actor: "Keanu Reeves", movies: [{title: "The Matrix", year: year}]}
  p year
end
(irb):55: warning: Pattern matching is experimental, and the behavior may change in future versions of Ruby!
1999
=> 1999

```

## Improved REPL (IRB)
Let's be honest. Even though irb is really helpful when we want to refresh some ruby syntax, it is not that fun to code on it. IRB now supports multi-line editing, syntax highlighting, inline editing of methods, auto completion and auto indentation.
<br>
<br>

![irb](https://media.giphy.com/media/W4bxGjRPzI5GsWViUD/giphy.gif)
<br>

## Beginless Range

A range without explicitly beginning is experimentally introduced.

```ruby
arr = ['a','b','c','d','a']
arr[..2]
#=> ['a','b','c']
```

## Enumerable#tally
It counts the occurrence of each element

```ruby
arr = ['a','b','c','d','a']

arr.tally

#=> {"a"=>2, "b"=>1, "c"=>1, "d"=>1}
```

## Argument Forwarding
This feature is quite useful when you want just to pass arguments from one parent function to his child. Ruby 2.7.0 made it easy with the argument forwarding (...)

```ruby
    def foo(...)
        Bar.new(...)
    end
```

Bear in mind that the parenthesis is mandatory.

## Separation of positional and keyword arguments

I believe that this is the biggest change brought by Ruby 2.7.0.
Now keyword argument is not considered a normal argument that is a Hash object passed as the last argument. From now on, a keyword is going to be treated as a separated argument like block is. 
<br>
If you run any ruby code that used the old of of arguments, it is going to show some warnings. This can be annoying especially when we run Rails because it may flood your console. Thanks to rails team, it is being fixed one by one as you can see here on this [issue](https://github.com/rails/rails/search?q=ruby+2.7&type=Issues) on github.

## How to Install?

You can install the new version by using your ruby package manager.
If you use `rbenv` you can install it by typing `rbenv install 2.7.0`
