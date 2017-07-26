# rails-good-to-know
Good-to-know when starting with Rails

# Proc & call

Proc objects are blocks of code that have been bound to a set of local variables. We can call the block of a Proc object with `call`

First thing first. A `proc`, an instance of Proc, needs a block to be created. If we try without a block :

```
> Proc.new
ArgumentError: tried to create Proc object without a block
in `new'
```

Good-to-know: `Proc.new` = `proc`

Let's create a proc!

```
Proc.new {|n| n*factor }
=> #<Proc:0x007fe6a64238c8@(pry):36>
```
We get a proc (an instance of Proc) ! :)

Now, let's have a look at the doc example.

```
def gen_times(factor)
  return Proc.new {|n| n*factor }
end
```

What happens if we call `gen_times(3)` ?

Think...

```
=> #<Proc:0x007fe6a62e0308@(pry):38>
```

We create a proc ! an instance of Proc ! To get the result of `n*factor`, we have to pass a parameter to the Proc block.

So to do this, we need to `call` the block !

```
gen_times(3).call(3)
=> 9
```
Yeah!

# Fetch

`fetch` returns a value from a hash for a given key. 
The big advantage of `fetch` is that if the key can't be found, it won't return nil, but it will raise an KeyError exception. 

```
h = { "a" => "awesome", "b" => "beautiful" }
```

```
h["a"]
=> "awesome"

h["z"]
=> nil
```

```
h.fetch("a")
=> "awesome"

h.fetch("z")
KeyError: key not found: "z"
```

It is helpful cause it specifies the error. We can also pass a default, that will be returned when the key can't be found.

```
h.fetch("z", "zyaouh!")
=> "zyaouh!"
```

We can also precise the error raising in a block. Look first how it looks without a block:

```
> def my_method(options:); end
=> :my_method

> my_method()
ArgumentError: missing keyword: options
in `my_method'

my_method(options: {})
=> nil
```

We define a method with mandatory options. If there is no param_1, we want to `raise 'Missing param_1' `

```
> def my_method(options:)
	options.fetch(:param_1) { raise 'Missing param_1' }
end
=> :my_method
```
Then when we call the method without `param_1`, we have:

```
> my_method(options: {})
RuntimeError: Missing param_1
in `block in my_method'
```
Then when we call the method with `param_1`, we have:

```
> my_method(options: {param_1: '2'})
=> "2"
```

# Difference between %W and %w
`%w` and `%W` are shortcuts to write array of strings separated by space, no need to bother with commas nor quotes

`%W[2*1 2*3] => ["2*1", "2*3"]`

The difference between both shortcuts is that `%W` (capital letter) allows interpolation

```
five = 5
six = 6
%W[2*1 2*3 #{five}*#{six}] => ["2*1", "2*3", "5*6"]
```

whereas `%w` doesn't:

```
five = 5
six = 6
%w[2*1 2*3 #{five}*#{six}] => ["2*1", "2*3", "\#{five}*\#{six}"]
```

Same for the %i and %I with symbols !

# Difference between `p` and `puts`

Let's define : 

```
string = "Hello"
array_string = ["Hello", "World"]
```

## PUTS

See what's happening with `puts`

```
puts string
# => Hello
puts array_string
# => Hello
# => World
```
It's not a string anymore...

## PUTS AND INSPECT

See what's happening with `puts .inspect`

```
puts string.inspect
# => "Hello"
puts array_string.inspect
# => ["Hello", "World"]
```

What about `p` ?

`puts .inspect` shows the same than `p` BUT doesn't return the same !

```
string = "Hello"

puts string
# > Hello
# => nil

puts string.inspect
# > "Hello"
# => nil

p string
# > "Hello"
# => "Hello"
```

So, we have to be careful when debugging with `puts`. Here is an example:

```
palindromes = ["kayak", "never odd or even", "ananas"].select { |word| word.reverse == word }
```
We expect to have as a result : (you can read them starting with both sides)
 
```
# => ["kayak", "never odd or even" ]
```
but we don't ! Instead we have :

```
# => ["kayak"]
```
So, we debug using `puts`

```
palindromes = ["kayak", "never odd or even", "ananas"].select { |word| puts(word.reverse) == word }
# > kayak
# > neve ro ddo reven
# > ananas
# > Result = []
```

Ok now, we see that `neve ro ddo reven` is not a palindromes because of the spaces. But here we see, `"kayak"` becomes `kayak`

So we CHANGED the behavior of the code using `puts`, now the result array is empty.

with a `p`, the behaviour of the code is not modified, so we still have `"kayak"` in our array of results
 

