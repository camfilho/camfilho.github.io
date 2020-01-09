---
layout: post
title: How to Make Ruby Constants Private
date: 2020-01-06 19:51:02 -0300
categories: [general]
tags: [ruby,rails]
description: 
---
## Straight to the Point
<br>
In order to make your Ruby constants not accessible to the outside world, Ruby gives us the `private_constant` method.
<br>
```ruby
class Phrase
  RE = REGULAR_EXPRESSION = { words: /\b[\w']+\b/ }.freeze
  private_constant :RE, :REGULAR_EXPRESSION

  private

  attr_reader :phrase

  def initialize(phrase)
    @phrase = phrase.downcase
  end

  def words
    phrase.scan(RE[:words])
  end

  public

  def word_count
    words.tally
  end
end
```
<br>
## Why

One main concern in OOP design is the balance between what to make accessible in a class's public interface and what to not. By default, Ruby makes all constants accessible to the outside world, and you can check the available constants by using the `#constants` method.

```ruby
irb(main): 001:0> Math.constants
=> [:DomainError, :PI, :E]
```
<br>
Wrapping information inside a constant can improve readability and reusability. For instance, if we just used the value `2.7182...` we may not notice that it stands for the Euler's number. However if `Math::E` is called, then chances are that the value is going to make sense.

In the first code snipped, a constant was used to hold the regexp value. Creating a regexp dictionary is a common practice to improve the it's readability. So now instead of using `phrase.scan(/\b[\w']+\b/)` and probably wouldn't easily remember, I can just use `phrase.scan(RE[:words]) and know that I am scanning for words.

Now, when we hit `ClassName.constants` our private constants won't show up and it is going to only be accessible to our private interface.

