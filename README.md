# cheatsheet.rb
A bridged Ruby cheatsheet, to remember the tricks that you may forget

Not everything that you can do in Ruby is considered good style. Functional style is highly favored, for instance the use of `each` or `map` instead of `while` or `for`. Piping functions is considered better and storing stage worst, though state should be stored in classes. Using a liner as Rubocop will help you writting code with better style quickly.

## Types
### String
```
'string'.upcase
'STRING'.downcase
```
### Boolean
```
# The double bang forces an expresion into boolean
!!nil
=> false
```

### Arrays
[Array](https://ruby-doc.org/core-3.0.0/Array.html) contains custom methods and the ones defined in the [Enumerable](https://ruby-doc.org/core-3.0.0/Enumerable.html) mixin. They contain specialized methods that are more functional in style and favored over using fors/loops and variables to store state
```
array = []
array = [true, 2, 'third']
array.each { |item| puts item }
array.each_with_index { |item, index| puts "item #{item} with index: #{index}" }

```

# Hashes
[Hash](https://ruby-doc.org/core-3.0.0/Hash.html) contains custom methods and the ones defined in the [Enumerable](https://ruby-doc.org/core-3.0.0/Enumerable.html) mixin. They contain specialized methods that are more functional in style and favored over using fors/loops and variables to store state
```
users = {
    carlos: 'admin',
    chuck: 'user',
    charles: 'provider'
}
puts users[:carlos] # Outputs: admin
empty_hash = Hash.new
another_empty_hash = {}

users.each_key { |key| puts key }
users.each_value { |value| puts value }
users.each { |key, value| puts "#{key}:#{value}" }

users.select { |key, value| value == 'admin' }

```

## Control Flow
```
# if-elseif-else
x = 1
if x == 2
  puts 'equals 2'
elseif x > 2
  puts 'bigger than 2'
else
  puts 'less than 2'
end

# Ternary operator (if-else)
puts x == 2 ? 'equals 2' : 'not equal to 2'

# inline unless
puts 'not equal to 2' unless x == 2

```

## Loops

```
# `for` statements are rarely used in Ruby where `each` is favored for iterating over all arguments
# `next` can be used to skip an iteration and `exit` to exit the loop

for x in 1..10
  break if x == 6
  next if x % 2 == 0
  break if x == 6
  puts x
end

Outputs:
1
3
5
=> nil

# `while` is rarely used too, it doesn't return the last evaluated block result
# `until` is analog to `while` and loops until the condition is true
x = 100
while(x != 50)
  x -= 1
end 
puts x

Outputs:
50
=> nil

5.times { puts 'one time' }
```

