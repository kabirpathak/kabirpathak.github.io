---
layout: post
title: "Ruby Splat(*) and Double Splat(**) Operators"
tldr: 
modified: 2024-09-27 16:03:24 +0530
category: tech
tags: [Ruby, Splat, Double Splat]
featured: true
rating: 5
author: kabir 
image: assets/images/tech/splat.png
---


Ruby offers powerful operators that allow flexibility when working with arguments in methods or assignments. Two of the most commonly used operators are the *splat* (`*`) and the *double splat* (`**`) operators. These operators make it easy to work with variable-length arguments and keyword arguments.

### Splat Operator (`*`) in Ruby
The splat operator (`*`) may seem confusing at first, but it is an incredibly powerful and flexible tool. Here’s how it works.

#### 1. Passing an Indefinite Number of Arguments to a Method
Normally, when we define a method in Ruby, we explicitly list the number of arguments it can accept. *For example:*

```ruby
def call_me(var1, var2, var3, var4, var5)
  puts "#{var1} #{var2} #{var3} #{var4} #{var5}"
end

call_me(1, 2, 3, 4, 5)  # Output: "1 2 3 4 5"
```

This works fine, but what if we don’t want to restrict the number of arguments that can be passed? Enter the splat operator:

```ruby
def call_me(*vars)
  puts vars.join(' ')
end

call_me(1, 2, 3, 4, 5)  # Output: "1 2 3 4 5"
call_me(1, 2)           # Output: "1 2"
call_me()               # Output: ""
```

In this example, the `*vars` collects all the arguments passed to the method into an array. You can pass any number of arguments (or none), and the method will handle it.

#### 2. Assignment of more than one value.
The splat operator can also be used in assignments. It allows us to assign multiple values from an array into variables. Here’s how it works:

```ruby
a, b, c = [1, 2, 3]
# a = 1, b = 2, c = 3
```

If the array has more values than variables, the extra values are discarded:

```ruby
a, b, c = [1, 2, 3, 4]
# a = 1, b = 2, c = 3  (4 is discarded)
```

Now, using the splat operator:

```ruby
a, b, *c = [1, 2, 3, 4]
# a = 1, b = 2, c = [3, 4]
```
In this case, `c` collects the remaining values in an array. This behavior makes it easy to manage arrays of different sizes.

Other variations of the splat operator in assignment include:

```ruby
a, *b, c = [1, 2, 3, 4]
# a = 1, b = [2, 3], c = 4

*a, b, c = [1, 2, 3, 4, 5, 6]
# a = [1, 2, 3, 4], b = 5, c = 6
```

As you can see, the splat operator can be very handy when dealing with arrays of different lengths, as it gathers multiple values into a single variable (in the form of an array).

#### 3. Using Splat for Array Expansion
You can also use the splat operator to expand arrays when passing them as arguments to methods. *For example:*

```ruby
def sum(a, b, c)
  a + b + c
end

numbers = [1, 2, 3]
puts sum(numbers[0], numbers[1], numbers[2]) # this can be rewritten as:
puts sum(*numbers)  # Output: 6
```

In this case, *numbers expands the array [1, 2, 3] into individual arguments for the sum method.

### Double Splat Operator (`**`) in Ruby
The double splat operator (`**`) is similar to the splat operator, but it is used specifically for keyword arguments in methods. Keyword arguments are passed as key-value pairs and are typically used when a method needs named arguments.

#### 1. Passing Keyword Arguments
Here’s an example of how the double splat operator works:

```ruby
def describe_person(**details)
  details.each do |key, value|
    puts "#{key.capitalize}: #{value}"
  end
end

describe_person(name: "John", age: 30, occupation: "Developer")
Output:

Name: John
Age: 30
Occupation: Developer
```

`**details` collects all keyword arguments passed to the method into a hash. You can pass any number of keyword arguments to the method, and they will be available as key-value pairs.

#### 2. Merging Hashes with Double Splat
The double splat operator is also useful when merging hashes, especially when dealing with keyword arguments in method calls.

```ruby
def display_options(**options)
  defaults = { font_size: 12, color: "black" }
  options = defaults.merge(options)
  puts options
end

display_options(font_size: 18, font_family: "Arial")
```

**Output:**

```irb
{:font_size=>18, :color=>"black", :font_family=>"Arial"}
```
In this example, the `display_options` method merges a default set of options with any keyword arguments passed by the user. The `**options` collects the keyword arguments into a hash, making it easy to handle them.

### Summary
The splat operator (`*`) is a powerful tool in Ruby that allows you to work with an indefinite number of arguments or assign multiple values in one go. It’s useful for collecting arguments into arrays and expanding arrays when passing them to methods.
The double splat operator (`**`) works similarly, but it’s specifically designed for handling keyword arguments. It collects keyword arguments into a hash and is useful when working with named arguments.
Both the splat and double splat operators add flexibility and readability to your Ruby code, making it easier to work with dynamic arguments and keyword-based options.
