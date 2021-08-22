# cheatsheet.rb
A bridged Ruby cheatsheet, to remember the tricks that you may forget

Not everything that you can do in Ruby is considered good style. Functional style is highly favored, for instance the use of `each` or `map` instead of `while` or `for`. Piping functions is considered better and storing stage worst, though state should be stored in classes. Using a liner as Rubocop will help you writting code with better style quickly.

## Types
### String
See [String documentation](https://ruby-doc.org/core-3.0.2/String.html#method-i-sub).
```
'string'.upcase
'STRING'.downcase
# returns string from standard input and removes /n and /at the end of the string
gets.chomp
# removes control chars and white spaces at the beggining and the end of the string
gets.strip
# Converting string to float or integer. Failure to conver returns zero
'1 ballon'.to_f => 1.0
'ballon'.to_i => 0

# sub substitutes the first occurence of a string or regex
'hello'.sub('e', '3')                        => "h3llo"
'hello'.sub(/[aeiou]/, '*')                  => "h*llo"
'hello'.sub(/([aeiou])/, '<\1>')             => "h<e>llo"
# gsub substitutes all occurrences
'hello'.gsub(/[aeiou]/, '*')                 => "h*ll*'
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
array[0] => true
array[index_out_of_range] => nil

# push and << are equivalent
array << 2
array.push(2)

# Sub arrays
array = [1, 2, 3, 4, 5]
# [index, length]
array[0,3]
# indexes as rage
array[0..3]
# until penultimate index
array[0..-2]


array.each { |item| puts item }
array.each_with_index { |item, index| puts "item #{item} with index: #{index}" }

array.map { |item| item.to_s }

```

### Hashes
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

# Case statements
case x
  when 1
    puts 'equals 1'
  when 2
    puts 'equals 2'
  else
    puts 'something else'
end

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
## Methods
### Yield
`yield` uses a block passed as an argument of a method call and executes it in the context of the method, having access to all the local variables.
```
def surround
  puts "{"
  yield
  puts "}"
end

surround { puts 'hola' }
```
### Splat Arguments
```
def splat_arguments(*array)
  puts array.length
end

splat_arguments('a','b',3) => 3
```

## Regular Expressions
You can test your regular expressions with [Rubilar](https://rubular.com/)

Quick reference:
```
[abc]	A single character of: a, b, or c
[^abc]	Any single character except: a, b, or c
[a-z]	Any single character in the range a-z
[a-zA-Z]	Any single character in the range a-z or A-Z
^	Start of line
$	End of line
\A	Start of string
\z	End of string
.	Any single character
\s	Any whitespace character
\S	Any non-whitespace character
\d	Any digit
\D	Any non-digit
\w	Any word character (letter, number, underscore)
\W	Any non-word character
\b	Any word boundary
(...)	Capture everything enclosed
(a|b)	a or b
a?	Zero or one of a
a*	Zero or more of a
a+	One or more of a
a{3}	Exactly 3 of a
a{3,}	3 or more of a
a{3,6}	Between 3 and 6 of a
```

Examples:
```
# string =~ regexp → integer or nil
"Do you like cats?" =~ /like/        => 7

# match(pattern, offset = 0) → matchdata or nilclick to toggle source
# match(pattern, offset = 0) {|matchdata| ... } → object
"Do you like cats?".match(/like/)          => #<MatchData "like">

url = 'https://docs.ruby-lang.org/en/2.5.0/MatchData.html'
m = url.match(/(\d\.?)+/)   # => #<MatchData "2.5.0" 1:"0">
m.string                    # => "https://docs.ruby-lang.org/en/2.5.0/MatchData.html"
m.regexp                    # => /(\d\.?)+/
# entire matched substring:
m[0]                        # => "2.5.0"

# Working with unnamed captures
m = url.match(%r{([^/]+)/([^/]+)\.html$})
m.captures                  # => ["2.5.0", "MatchData"]
m[1]                        # => "2.5.0"
m.values_at(1, 2)           # => ["2.5.0", "MatchData"]

# Working with named captures
m = url.match(%r{(?<version>[^/]+)/(?<module>[^/]+)\.html$})
m.captures                  # => ["2.5.0", "MatchData"]
m.named_captures            # => {"version"=>"2.5.0", "module"=>"MatchData"}
m[:version]                 # => "2.5.0"
m.values_at(:version, :module)
                            # => ["2.5.0", "MatchData"]
# Numerical indexes are working, too
m[1]                        # => "2.5.0"
m.values_at(1, 2)           # => ["2.5.0", "MatchData"]

```
