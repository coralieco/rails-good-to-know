# rails-good-to-know
Good-to-know when starting with Rails

# Difference between %W and %w
`%w` and `%W` are shortcuts to write array of strings separated by space, no need to write commas nor quotes around them

`%W[2*1 2*3] => ["2*1", "2*3"]`

The difference between both shortcuts is that `%W` allows interpolation

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


