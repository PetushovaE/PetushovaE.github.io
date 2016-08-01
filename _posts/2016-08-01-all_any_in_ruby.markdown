---
layout: post
title:  "All? any? in Ruby"
date:   2016-08-01 09:05:18 -0400
---


In my first blog, I’ll cover two of Enumerable‘s methods: all? and any? methods we’re guaranteed to use in our day-to-day Ruby development: Enumerable, a mixin that adds traversal, search, filtering and sorting functionality to collection classes, including those workhorses known as Array and Hash. 

With all? and any? you can check, if all or any elements of an Array or Hash (or any other class that includes the Enumerable module) match a certain criteria. The result is either true or false.

**all?: **
(Do all the items in the collection meet the given criteria?)
The method returns true if the block NEVER returns false or nil.


```
["ant", "bear", "cat"].all? { |word| word.length >= 3 } #=> true
["ant", "bear", "cat"].all? { |word| word.length >= 4 } #=> false
```


**Using all? with Arrays**

When used on an array and a block is provided, all? passes each item to the block. If the block never returns false or nil during this process, all? returns true; otherwise, it returns false.

**Using all? with Hashes**

When used on a hash and a block is provided, all? passes each key/value pair in the hash to the block, which you can “catch” as either:

A two-element array, with the key as element 0 and its corresponding value as element 1, or
Two separate items, with the key as the first item and its corresponding value as the second item.
If the block never returns false or nil during this process, all? returns true; otherwise, it returns false.

```
languages = {"Javascript" => 1996, "PHP" => 1994, "Perl" => 1987, "Python" => 1991, "Ruby" => 1993}
 
languages.all? {|language| language[0] >= "Delphi"} => true
 
languages.all? {|language, year_created| language >= "Delphi"} => true
 
languages.all? {|language| language[0] >= "Visual Basic"} => false
 
languages.all? {|language, year_created| language >= "Visual Basic"} => false
 
languages.all? {|language| language[0] >= "Delphi" and language[1] <= 2000} => true
 
languages.all? {|language, year_created| language >= "Delphi" and year_created <= 2000} => true
 
languages.all? {|language| language[0] >= "Delphi" and language[1] > 2000} => false
 
languages.all? {|language, year_created| language >= "Delphi" and year_created > 2000} => false
```


Using all? without a block on a hash is meaningless, as it will always return true. When the block is omitted, all? uses this implied block: {|item| item}. In the case of a hash, item will always be a two-element array, which means that it will never evaluate as false nor nil.

And yes, even this hash, when run through all?, will still return true:

```
{false => false, nil => nil}.all? => true
```

**any?:**
(Do any of the items in the collection meet the given criteria?)
The method returns true if the block EVER returns a value other than false or nil.

```
["ant", "bear", "cat"].any? { |word| word.length >= 3 } #=> true
["ant", "bear", "cat"].any? { |word| word.length >= 4 } #=> true
```

**Using any? with Arrays**

When used on an array and a block is provided, any? passes each item to the block. If the block returns true for any item during this process, any? returns true; otherwise, it returns false.

```
cheeses = ["feta", "cheddar", "stilton", "camembert", "mozzarella", "Fromage de Montagne de Savoie"]
 
cheeses.any? {|cheese| cheese.length >= 25} => true
 
cheeses.any? {|cheese| cheese.length >= 35} => false
```

When the block is omitted, any? uses this implied block: {|item| item}. Since everything in Ruby evaluates to true except for false and nil, using any? without a block is effectively a test to see if any of the items in the collection evaluate totrue (or conversely, if all the values in the array evaluate to false or nil).
cheeses.any?
=> true

``` 
cheeses = [false, nil] => [false, nil]
 
cheeses.any? => false
```

Remember that in Ruby, everything expect for false and nil evaluates to true:
cheeses << 0 => [false, nil, 0]
 
`>> cheeses.any? => true`

**Using any? with Hashes**
When used on a hash and a block is provided, any? passes each key/value pair in the hash to the block, which you can “catch” as either:

A two-element array, with the key as element 0 and its corresponding value as element 1, or
Two separate items, with the key as the first item and its corresponding value as the second item.
If the block returns true for any item during this process, any? returns true; otherwise, it returns false.

```
languages = {"Javascript" => 1996, "PHP" => 1994, "Perl" => 1987, "Python" => 1991, "Ruby" => 1993}
 
languages.any? {|language| language[0] < "Pascal"} => true
 
languages.any? {|language, year_created| language < "Pascal"} => true
 
languages.any? {|language| language[0] < "Fortran"} => false
 
languages.any? {|language, year_created| language < "Fortran"} => false
 
languages.any? {|language| language[0] >= "Basic" and language[1] <= 1995} => true
 
languages.any? {|language, year_created| language >= "Basic" and year_created <= 1995} => true
 
languages.any? {|language| language[0] >= "Basic" and language[1] <= 1985} => false
 
languages.any? {|language, year_created| language >= "Basic" and year_created <= 1985} 
=> false
```

**plus:**

`[].all? #=> true`
So since the block never gets called, it never returns false or nil, thus all returns true.

`[].any? #=> false`
Since the block never gets called, it never returns a value other than false or nil, thus any returns false.

we wanted to do something more complicated, like determine whether there were any even numbers in the array?
`[1, 2, 3].any? { |number| number % 2 == 0 } # => true`
`[1, 3, 5].any? { |number| number % 2 == 0 } # => false`
The any? method says: are there any elements in the array for which this block evaluates to true? We might also want to know whether all? of the elements were even.

`[2, 4, 6].all? { |number| number % 2 == 0 } # => true`
`[1, 2, 3].all? { |number| number % 2 == 0 } # => false`
We can see that making good use of Enumerable methods produces highly readable code.

Conveniently, Enumerable provides similar methods for extracting objects. What if we wanted, for example, to retrieve all of the even numbers.

`[1, 2, 3, 4].select { |number| number % 2 == 0 } # => [2, 4]`
...or, the first even number...

`[1, 2, 3, 4].detect { |number| number % 2 == 0 } # => 2`


Finally, in order to take your Enumerable usage to the next level, there are a few more methods you should have in your toolbox:
-map/collect
-inject (known in other languages as fold)
-partition
-grep





