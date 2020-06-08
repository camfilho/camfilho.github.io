---
layout: post
title: How To Add Attribute Name and Value to Validation
date: 2020-06-08 19:51:02 -0300
categories: [general]
tags: [ruby,rails]
description: Working with Validation Messages
---
## Validations?
<br>
Validations are an application-level method to ensure consistency on your database by only allowing valid data to be written on it.

Rails chooses to write validations in the Model by default to keep the [fat modal, skinny controller approach](https://riptutorial.com/ruby-on-rails/example/9609/fat-model--skinny-controller).
<br>

## :message
By using the `:message` option, Rails allows us to give the user a custom error message. `:message` accepts a `string` or a `proc` to define its content. Once the validation is triggered, the message is added to the error collection and possibly sent to the user through a flash message by the controller or simply as an api return.<br>

### `string`
If a simple string is enough to express the error, `Rails` provides `%{value}, %{attribute} and %{model} to use as a string-interpolation so it is dynamically replaced when validation fails.

```ruby
  validates :amount, numericality: { message: "%{value} seems to not be a number" }
```

In the above sample, the value of the atttribute that triggered the validation is added inside the message to the error collection. 

### `Proc`
If you need a more robust and flexible error message, it is possible to do so by applying proc.
The `:message`'s proc is given two arguments, the Object being validated and a hash with `:modal`, `:attribute` and `:value` key-value pair.
A simple use of a Proc as a message would be:
```ruby
  validates :email, 
    uniqueness: {
      message: ->(object, data) do
        "The email of value, #{data[:value]} is taken already! Try again #{Time.zone.tomorrow}"
      end
    }
```

You may be wondering why bother with Proc since we already accomplished the same with string interpolation.<br>
If, by any chahnce, you want to display additional information for some users but not for another, you could accomplish this by putting a conditional inside the `Proc`:
```ruby
   validates :email, 
    uniqueness: {
      message: ->(object, data) do
        if object.internal?
          "The email of value, #{data[:value]} is taken already! Try again #{Time.zone.tomorrow}"
        else
          "You may not need to do this"
        end
      end
    }
```
## Exercise Time

This one is easy, write a simple validation for your mode. Try to print the value of the attribute that you are trying to validate by using `string interpolation`.
<br>
Do the same by using `Proc`, but now add a `debugger` using `byebug` so you can inspect the values of `object` and `data` arguments of your `Proc`.

<br>
Happy Coding!