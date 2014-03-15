---
layout: post
title: Star in Ruby
date: 2014-03-15 00:11
comments: true
categories: 
published: true
---

I have seen the Ruby's `*` operator used at several places in different
ways and often times I get confused the way it is used. Finally I decided to learn more about it and here are few interesting roles of `*` operator that I found in Ruby.



---

### Multiplication

This is most obvious role of `*`. It's the multiplication operator. `2 * 2 = 4`

Or an instance method of `Fixnum` class to perform multiplication. `2.*(2) = 4`

<!-- more -->
---

### Repetition

Several classes in Ruby defines `*` operator as a Repetition method.

* `Array` class defines `*` for two different possible type of parameters.

```
ary * int -> new_ary
ary * str -> new_str
```

For string type it acts as a `join` method. Examples:

```rb
[1,2,3] * 3 # => [1, 2, 3, 1, 2, 3, 1, 2, 3]

[1,2,3] * ',' # => "1,2,3"

```

* `String` class defined `*` as a **copy** operator. It returns a new string
containing n copies of the string. For example:

```rb

"ho! " * 3 # => "ho! ho! ho! "

```
Note that `String` `*` `str` is not defined.


---

### Splat

Another less known use of `*` is as a **splat** operator. Consider
following example where we want to convert options_array to
options_hash.

```rb
options_array = [:first_name, "Ankit", :last_name, "Goyal"]
# to
options_hash = {:first_name => "Ankit", :last_name => "Goyal"}

```

You will see that [`[]`]( http://www.ruby-doc.org/core-2.1.0/Hash.html#method-c-5B-5D ) method in `Hash` class takes [key, value, ...] as an argument and
creates a Hash for you. For example:
```
> Hash[1,2,3,4]
# {1 => 2, 3 => 4}
```

So how can we use `[]` method to get the required hash?  You can see in
documentation that `[]` doesn't
take array as a parameter or you can try running it. It will return you an empty hash with lot of warnings. So we can't call `Hash[options_array]` directly. 

We can use splat `*` operator to convert our array into arguments. If
you call `[]` with `*options_array` as a parameter, you'll get the
required hash back.

```rb
options_hash = Hash[*options_array]
# {:first_name => "Ankit", :last_name => "Goyal"}
```

So splat converts `*options_array` to an argument list. Let's see some
other interesting uses of splat operator.

**Splat** can be used to convert an array to argument list as shown above. You can call methods by dynamically generating arguments with minimal
amount of code. For example, if you have a **string** with ',' separated
values of arguments.


```
def do_it(arg1, arg2, arg3)
    p "#{arg1} #{arg2} #{arg3}"
end

arguments = "arg1,arg2,arg3"
```

You can call `do_it` method with `arg1`, `arg2` and `arg3` as arguments
using splat like this:

```
do_it(*arguments.split(','))
# => "arg1 arg2 arg3"
```

You can convert list of arguments back to array using splat. For
example:

```
*argument_array = 1, 2, 3
> argument_array
# => [1, 2, 3]
```

**Note** that you don't need to use splat for the above code to work. 
```
argument_array = 1, 2, 3
> argument_array
# => [1, 2, 3]
```
is equally valid. But it's good to know that you can do it using splat
also.
