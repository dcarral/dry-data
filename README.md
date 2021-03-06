# Dry::Data

A simple type-system for Ruby respecting ruby's built-in coercion mechanisms.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'dry-data'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install dry-data

## Why?

Unlike seemingly similar libraries like virtus, attrio, fast_attrs, attribs etc.
`Dry::Data` provides you an interface to explicitly specify data types you want
to use in your application domain which gives you type-safety and *simple* coercion
mechanism using built-in coercion methods on the kernel.

Main difference is that `Dry::Data` is not designed to handle all kinds of complex
coercions that are typically required when dealing with, let's say, form params
in a web application. Its primary focus is to allow you to specify the exact shape
of the custom application data types to avoid silly bugs that are often hard to debug
(`NoMethodError: undefined method `size' for nil:NilClass` anyone?).

## Usage

Primary usage of this library is defining domain data types that your application
will work with. The interface consists of lower-level type definitions and a higher-level
virtus-like interface for defining structs.


### Defining types

For now the usage is very simple, more features will be built upon this interface:

``` ruby
string = Dry::Data.new { |t| t['String'] }
array = Dry::Data.new { |t| t['Array'] }

string[:foo] # => 'foo'
array[:foo] # => [:foo]
```

### Defining a struct

``` ruby
class User
  include Dry::Data::Struct

  attributes name: String, age: Integer
end

# becomes available like any other type
user_type = Dry::Data.new { |t| t['User'] }

user = user_type[name: :Jane, age: '21']

user.name # => "Jane"
user.age # => 21
```

## WIP

This is early alpha with a rough plan to:

* Add constrained types (ie a string with a strict length, a number with a strict range etc.)
* Add support for `Optional`/`Maybe` type to be able to explicitly specify that a given value could be nil (aka ActiveSupport `try` done right)
* Benchmark against other libs and make sure it's fast enough

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/[USERNAME]/dry-data.

