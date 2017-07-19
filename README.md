# rails-good-to-know
Good-to-know when starting with Rails

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
 

