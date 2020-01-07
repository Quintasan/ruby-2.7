# Ruby 2.7

---

## Summary

@ul[spaced]
- Pattern Matching
- REPL improvement
- Separation of positional and keyword arguments
- Compaction GC
- Other stuff
@ulend

---

## Pattern Matching

```ruby
require "json"

json = <<END
{
  "name": "Alice",
  "age": 30,
  "children": [{ "name": "Bob", "age": 2 }]
}
END

case JSON.parse(json, symbolize_names: true)
in {name: "Alice", children: [{name: "Bob", age: age}]}
  p age #=> 2
end
```

---

## Better IRB

![Improved REPL demo video](https://cache.ruby-lang.org/pub/media/irb_improved_with_key_take3.mp4)

---

@snap[west span-50]
## Better IRB
@snapend

@snap[east span-50]
@ul
- Syntax highlighting
- Multiline mode
- Show documentation with double Tab
- Auto-indentation
- Save and load history by default
- Still not as good as Pry :(
@ulend
@snapend

---

## Separation of positional and keyword arguments

Automatic conversion of keyword arguments and positional arguments is d e p r e c a t e d.

---

@snap[north span-40]
### Definition
```ruby
def foo(key: 42); end
def foo(**kw); end
```
@snapend

@snap[west span-50]
### Good
```ruby
foo(**{key: 42}) ## OK
```
@snapend

@snap[east span-50]
### Bad
```ruby
foo({key:42}) ## warn
```
@snapend

---

@snap[north span-40]
### Definition
```ruby
def foo(h, key: 42); end
def foo(h, **kw); end
```
@snapend

@snap[west span-50]
### Good
```ruby
foo({key:42}) ## OK
```
@snapend

@snap[east span-50]
### Bad
```ruby
foo(key:42) ## warn
```
@snapend

---

@snap[north span-40]
### Definition
```ruby
def foo(h = {}, key: 42); end
```
@snapend

@snap[west span-50]
### Good
```ruby
foo({"key" => 43}, key: 42) # OK
```
@snapend

@snap[east span-50]
### Bad
```ruby
foo("key" => 43, key: 42) # warn
foo({"key" => 43, key: 42}) # warn
```
@snapend

---

## Splat nil

```ruby
def foo(h, **nil); end

foo(key: 1) # ArgumentError
foo(**{key: 1}) # ArgumentError
foo("str" => 1) # ArgumentError
foo({key: 1} # OK
foo({"str" => 1} # OK
```

---

## Numbered parameters

Of course, people are [already whining](https://bugs.ruby-lang.org/issues/15723) about it.

```ruby
(1..10).each { puts _1 }
```

---

## Enumerable#tally

This is the feature of the year.

```ruby
%w[1 1 1 2 2 3 3 4 4 4 5].tally
#=> {"1" => 3, "2" => 2, "3" => 2, "4" => 3, "5" => 1 }
```

---

## Other syntax changes

---

## Comments between method chaining

```ruby
(1..20).select(&:odd?)
       # This was not possible in 2.6
       .map { _1**2 }
```

---

## Flip-flop is back!

```ruby
selected = []

0.upto 10 do |value|
  selected << value if value==2..value==8
end

p selected
#=> [2, 3, 4, 5, 6, 7, 8]
```

---

## Other stuff

@ul[spaced]
- Bundler 2.1.2
- RubyGems 3.1.2
- Unicode 12.1.0
- **Require compilers to support C99**
@ulend

## Even more stuff

See [RubyReferences](https://rubyreferences.github.io/rubychanges/2.7.html)

## That's all
