# `puts "magnetis"`

Magnetis Ruby code styleguide.

## Table of Contents

* [Disclaimer](#disclaimer)
* [Code layout](#code-layout)
* [Syntax](#syntax)
* [Naming](#naming)
* [Comments](#comments)
* [Annotations](#annotations)
* [Classes](#classes)
* [Exceptions](#exceptions)
* [Collections](#collections)
* [Strings](#strings)
* [Regular Expressions](#regular-expressions)
* [Percent Literals](#percent-literals)
* [Kernel methods](#kernel-methods)
* [Metaprogramming](#metaprogramming)
* [Misc](#misc)

## Disclaimer

This document is based on the [Bozhidar Batsov's ruby style guide](https://github.com/bbatsov/ruby-style-guide) and adapted to our preferences.
It is compatible with Ruby >= 2.1.

## Code layout

* Use `UTF-8` as the source file encoding.
* Use two **spaces** per indentation level. No hard tabs.

    ```ruby
    # good
    def some_method
      do_something
    end

    # bad - four spaces
    def some_method
        do_something
    end
    ```

* Use spaces around operators, after commas, colons and semicolons, around `{`
  and before `}`. This ensure a more readable code.

    ```ruby
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? true : false; puts 'Hi'
    [1, 2, 3].each { |e| puts e }
    ```

* No spaces after `(`, `[` or before `]`, `)`.

    ```ruby
    some(arg).other
    [1, 2, 3].length
    ```

* Indent `when` as deep as `case`.

    ```ruby
    case
    when song.name == 'Misty'
      puts 'Not again!'
    when song.duration > 120
      puts 'Too long!'
    when Time.now.hour > 21
      puts "It's too late"
    else
      song.play
    end

    kind = case year
           when 1850..1889 then 'Blues'
           when 1890..1909 then 'Ragtime'
           when 1910..1929 then 'New Orleans Jazz'
           when 1930..1939 then 'Swing'
           when 1940..1950 then 'Bebop'
           else 'Jazz'
           end
    ```

* Use empty lines between `def`s and to break up a method into logical
  paragraphs.

    ```ruby
    def some_method
      data = initialize(options)

      data.manipulate!

      data.result
    end

    def some_method
      result
    end
    ```

* Align the parameters of a method call if they span over multiple lines.

    ```ruby
    # starting point (line is too long)
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
    end

    # bad
    def send_mail(source)
      Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text)
    end

    # good
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com',
                     from: 'us@example.com',
                     subject: 'Important message',
                     body: source.text)
    end
    ```

* Use [TomDoc](http://tomdoc.org/) and its conventions for API documentation.
* Keep lines fewer than **100** characters.
* Avoid trailing whitespace.

## Syntax

* Avoid line continuation (\\) where not required. In practice, avoid using
  line continuations at all.

    ```ruby
    # bad
    result = 1 - \
             2

    # good (but still ugly as hell)
    result = 1 \
             - 2
    ```

* Using the return value of `=` (an assignment) is ok, but surround the
  assignment with parentheses.

    ```ruby
    # good - shows intended use of assignment
    if (v = array.grep(/foo/)) ...

    # bad
    if v = array.grep(/foo/) ...

    # also good - shows intended use of assignment and has correct precedence.
    if (v = self.next_value) == 'hello' ...
    ```

* Use `||=` freely to initialize variables.

    ```ruby
    # set name to Bozhidar, only if it's nil or false
    name ||= 'Bozhidar'
    ```

* Don't use `||=` to initialize boolean variables. (Consider what
  would happen if the current value happened to be `false`.)

    ```ruby
    # bad - would set enabled to true even if it was false
    enabled ||= true

    # good
    enabled = true if enabled.nil?
    ```

* The new hash literal syntax is preferred when your hash keys are symbols.

    ```ruby
    # bad
    hash = { :one => 1, :two => 2 }

    # good
    hash = { one: 1, two: 2 }
    ```

* Use `_` for unused method/block parameters.

    ```ruby
    # bad
    result = hash.map { |k, v| v + 1 }

    # good
    result = hash.map { |_, v| v + 1 }
    ```

* Use `_` followed by a name for unused method/block parameters.

    ```ruby
    # bad
    def calculate(value)
      21 + 21
    end

    # good
    def calculate(_value)
      21 + 21
    end
    ```

### Methods

* Use `def` with parentheses when there are arguments. Omit the
  parentheses when the method doesn't accept any arguments.

     ```ruby
     # bad
     def some_method()
       # body omitted
     end

     # good
     def some_method
       # body omitted
     end

     # bad
     def some_method_with_arguments arg1, arg2
       # body omitted
     end

     # good
     def some_method_with_arguments(arg1, arg2)
       # body omitted
     end
     ```

* Omit parentheses around parameters for methods that are part of an
  internal DSL (e.g. Rake, Rails, RSpec).

    ```ruby
    class Person

      # bad
      attr_reader(:name, :age)

      # good
      attr_reader :name, :age

      # omitted
    end

    temperance = Person.new('Temperance', 30)
    temperance.name

    # bad
    puts temperance.age()

    # good
    puts temperance.age

    x = Math.sin(y)

    # bad
    array.delete e

    # good
    array.delete(e)
    ```

* Avoid `self` where not required. (It is only required when calling a self write accessor.)

    ```ruby
    # bad
    def ready?
      if self.last_reviewed_at > self.last_updated_at
        self.worker.update(self.content, self.options)
        self.status = :in_progress
      end
      self.status == :verified
    end

    # good
    def ready?
      if last_reviewed_at > last_updated_at
        worker.update(content, options)
        self.status = :in_progress
      end

      status == :verified
    end
    ```

* Avoid shadowing methods with local variables unless they are both equivalent.

    ```ruby
    class Foo
      attr_accessor :options

      # ok
      def initialize(options)
        self.options = options
        # both options and self.options are equivalent here
      end

      # bad
      def do_something(options = {})
        unless options[:when] == :later
          output(self.options[:message])
        end
      end

      # good
      def do_something(params = {})
        unless params[:when] == :later
          output(options[:message])
        end
      end
    end
    ```

* Use spaces around the `=` operator when assigning default values to method parameters:

    ```ruby
    # bad
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end

    # good
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end
    ```

* Never put a space between a method name and the opening parenthesis.

    ```ruby
    # bad
    f (3 + 2) + 1

    # good
    f(3 + 2) + 1
    ```

* If the first argument to a method begins with an open parenthesis,
  always use parentheses in the method invocation. For example, write
`f((3 + 2) + 1)`.

### Loops

* Never use `for`, unless you know exactly why.

    ```ruby
    arr = [1, 2, 3]

    # bad
    for elem in arr do
      puts elem
    end

    # good
    arr.each { |elem| puts elem }
    ```

* Favor modifier `while/until` usage when you have a single-line
  body.

    ```ruby
    # bad
    while some_condition
      do_something
    end

    # good
    do_something while some_condition
    ```

* Favor `until` over `while` for negative conditions.

    ```ruby
    # bad
    do_something while !some_condition

    # good
    do_something until some_condition
    ```

* Prefer `{...}` over `do...end` for single-line blocks.  Avoid using
  `{...}` for multi-line blocks (multiline chaining is always
  ugly). Always use `do...end` for "control flow" and "method
  definitions" (e.g. in Rakefiles and certain DSLs).  Avoid `do...end`
  when chaining.

    ```ruby
    names = ['Bozhidar', 'Steve', 'Sarah']

    # good
    names.each { |name| puts name }

    # bad
    names.each do |name|
      puts name
    end

    # bad
    names.each { |name|
      puts name
      puts name.upcase
    }

    # good
    names.each do |name|
      puts name
      puts name.upcase
    end

    # good
    names.select { |name| name.start_with?('S') }.map { |name| name.upcase }

    # bad
    names.select do |name|
      name.start_with?('S')
    end.map { |name| name.upcase }
    ```

### Conditionals

* Never use `then` for `if/unless`.

    ```ruby
    # bad
    if some_condition then
      # body omitted
    end

    # good
    if some_condition
      # body omitted
    end
    ```

* Favor the ternary operator(`?:`) over `if/then/else/end` constructs for one line conditionals.

    ```ruby
    # bad
    result = if some_condition then something else something_else end

    # good
    result = some_condition ? something : something_else
    ```

* Use one expression per branch in a ternary operator. This
  also means that ternary operators must not be nested. Prefer
  `if/else` constructs in these cases.

    ```ruby
    # bad
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # good
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```

* Never use `if x; ...`. Use the ternary operator instead.
* Avoid multi-line `?:` (the ternary operator); use `if/unless` instead.
* Use `when x then ...` for one-line cases.
* Use `&&/||` for boolean expressions, `and/or` for control flow.  (Rule
  of thumb: If you have to use outer parentheses, you are using the
  wrong operators.)

    ```ruby
    # boolean expression
    if some_condition && some_other_condition
      do_something
    end

    # control flow
    document.saved? or document.save!
    ```

* Favor modifier `if/unless` usage when you have a single-line body.

    ```ruby
    # bad
    if some_condition
      do_something
    end

    # good
    do_something if some_condition
    ```

* Favor `unless` over `if` for negative conditions.

    ```ruby
    # bad
    do_something if !some_condition

    # good
    do_something unless some_condition
    ```

* Never use `unless` with `else`. Rewrite these with the positive case first.

    ```ruby
    # bad
    unless success?
      puts 'failure'
    else
      puts 'success'
    end

    # good
    if success?
      puts 'success'
    else
      puts 'failure'
    end
    ```

* Don't use parentheses around the condition of an `if/unless/while`,
  unless the condition contains an assignment (see "Using the return
  value of `=`" below).

    ```ruby
    # bad
    if (x > 10)
      # body omitted
    end

    # good
    if x > 10
      # body omitted
    end

    # ok
    if (x = self.next_value)
      # body omitted
    end
    ```

* Avoid `return` where not required for flow of control.

    ```ruby
    # bad
    def some_method(some_arr)
      return some_arr.size
    end

    # good
    def some_method(some_arr)
      some_arr.size
    end
    ```

* Avoid the usage of `and` and `or` as control flow. They may have unexpected results.

    ```ruby
    # bad
    if x and y
      # body omitted
    end

    # good
    if x && y
      # body omitted
    end

    # bad
    if x or y
      # body omitted
    end

    # good
    if x || y
      # body omitted
    end
    ```

## Naming

* Use `snake_case` for methods and variables.
* Use `CamelCase` for classes and modules.
* Keep acronyms like HTTP, RFC, XML uppercase.
* Use `SCREAMING_SNAKE_CASE` for other constants.
* The names of predicate methods (methods that return a boolean value)
  should end in a question mark.
  (i.e. `Array#empty?`).
* The names of potentially "dangerous" methods (i.e. methods that modify `self` or the
  arguments, `exit!` (doesn't run the finalizers like `exit` does), etc.) should end with an exclamation mark if
  there exists a safe version of that *dangerous* method.

    ```ruby
    # bad - there is not matching 'safe' method
    class Person
      def update!
      end
    end

    # good
    class Person
      def update
      end
    end

    # good
    class Person
      def update!
      end

      def update
      end
    end
    ```

* Define the non-bang (safe) method in terms of the bang (dangerous)
  one if possible.

    ```ruby
    class Array
      def flatten_once!
        res = []

        each do |e|
          [*e].each { |f| res << f }
        end

        replace(res)
      end

      def flatten_once
        dup.flatten_once!
      end
    end
    ```

* When using `reduce` with short blocks, name the arguments `|a, e|`
  (accumulator, element).
* When defining binary operators, name the argument `other`.

    ```ruby
    def +(other)
      # body omitted
    end
    ```

* Prefer `map` over `collect`, `find` over `detect`, `select` over
  `find_all`, `reduce` over `inject` and `size` over `length`. This is
  not a hard requirement; if the use of the alias enhances
  readability, it's ok to use it. The rhyming methods are inherited from
  Smalltalk and are not common in other programming languages. The
  reason the use of `select` is encouraged over `find_all` is that it
  goes together nicely with `reject` and its name is pretty self-explanatory.

## Comments

> Good code is its own best documentation. As you're about to add a
> comment, ask yourself, "How can I improve the code so that this
> comment isn't needed?" Improve the code and then document it to make
> it even clearer. <br/>
> <small>- _Steve McConnell_</small>

* Write self-documenting code and ignore the rest of this section. Seriously!
* Avoid superfluous comments.

    ```ruby
    # bad
    counter += 1 # increments counter by one
    ```

* Keep existing comments up-to-date. An outdated is worse than no comment at all.
* Avoid writing comments to explain bad code. Refactor the code to
  make it self-explanatory. (Do or do not - there is no try. --Yoda)

## Annotations

* Annotations should usually be written on the line immediately above
  the relevant code.
* The annotation keyword is followed by a colon and a space, then a note
  describing the problem.
* If multiple lines are required to describe the problem, subsequent
  lines should be indented below the text `#`.

    ```ruby
    def bar
      # FIXME: This has crashed occasionally since v3.2.1. It may
      #        be related to the BarBazUtil upgrade.
      baz(:quux)
    end
    ```

* In cases where the problem is so obvious that any documentation would
  be redundant, annotations may be left at the end of the offending line
  with no note. This usage should be the exception and not the rule.

    ```ruby
    def bar
      sleep 100 # OPTIMIZE
    end
    ```

* Use `TODO` to note missing features or functionality that should be
  added at a later date.
* Use `FIXME` to note broken code that needs to be fixed.
* Use `OPTIMIZE` to note slow or inefficient code that may cause
  performance problems.
* Use `HACK` to note code smells where questionable coding practices
  were used and should be refactored away.
* Use `REVIEW` to note anything that should be looked at to confirm it
  is working as intended. For example: `REVIEW: Are we sure this is how the
  client does X currently?`
* Use other custom annotation keywords if it feels appropriate, but be
  sure to document them in your project's `README` or similar.

## Classes

* When designing class hierarchies make sure that they conform to the
  [Liskov Substitution Principle](http://en.wikipedia.org/wiki/Liskov_substitution_principle).
* Try to make your classes as
  [SOLID](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design\))
  as possible.
* Always try to supply a proper `to_s` method for classes that represent domain objects.

    ```ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def to_s
        "#{first_name} #{last_name}"
      end
    end
    ```

* Use the `attr` family of functions to define trivial accessors or
mutators.

    ```ruby
    # bad
    class Person
      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def first_name
        @first_name
      end

      def last_name
        @last_name
      end
    end

    # good
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end
    ```

* Consider using `Struct.new`, which defines the trivial accessors,
constructor and comparison operators for you.

    ```ruby
    # good
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end

    # better
    class Person < Struct.new(:first_name, :last_name)
    end
    ````

* Consider adding factory methods to provide additional sensible ways
to create instances of a particular class.

    ```ruby
    class Person
      def self.create(options_hash)
        # body omitted
      end
    end
    ```

* Prefer [duck-typing](http://en.wikipedia.org/wiki/Duck_typing) over inheritance.

    ```ruby
    # bad
    class Animal
      # abstract method
      def speak
      end
    end

    # extend superclass
    class Duck < Animal
      def speak
        puts 'Quack! Quack'
      end
    end

    # extend superclass
    class Dog < Animal
      def speak
        puts 'Bau! Bau!'
      end
    end

    # good
    class Duck
      def speak
        puts 'Quack! Quack'
      end
    end

    class Dog
      def speak
        puts 'Bau! Bau!'
      end
    end
    ```

* Avoid the usage of class (`@@`) variables due to their "nasty" behavior in inheritance.

    ```ruby
    class Parent
      @@class_var = 'parent'

      def self.print_class_var
        puts @@class_var
      end
    end

    class Child < Parent
      @@class_var = 'child'
    end

    Parent.print_class_var # => will print "child"
    ```

    As you can see all the classes in a class hierarchy actually share one
    class variable. Class instance variables should usually be preferred
    over class variables.

* Assign proper visibility levels to methods (`private`, `protected`)
  in accordance with their intended usage. Don't go off leaving
  everything `public` (which is the default).

* Indent the `public`, `protected`, and `private` methods as much the
  method definitions they apply to. Leave one blank line above and below the visibility name.

    ```ruby
    class SomeClass
      def public_method
        # ...
      end

      private

      def private_method
        # ...
      end
    end
    ```

* Always declare private attribute right after the `private` keyword.
    ```ruby
    class Foo
      # body omitted

      private

      def some_method
      end

      # bad
      attr_reader :bar, baz
    end
    ```

    ```ruby
    class Foo
      # body omitted

      private

      # good
      attr_reader :bar, baz

      def some_method
      end
    end
    ```

* Use `def self.method` to define singleton methods. This makes the code
  easier to refactor since the class name is not repeated.

    ```ruby
    class TestClass
      # bad
      def TestClass.some_method
        # body omitted
      end

      # good
      def self.some_other_method
        # body omitted
      end

      # Also possible and convenient when you
      # have to define many singleton methods.
      class << self
        def first_method
          # body omitted
        end

        def second_method_etc
          # body omitted
        end
      end
    end
    ```

## Exceptions

* Signal exceptions using the `fail` keyword. Use `raise` only when
  catching an exception and re-raising it
  (because here you're not failing, but explicitly and purposefully raising an exception).

    ```ruby
    begin
      fail 'Oops';
    rescue => error
      raise if error.message != 'Oops'
    end
    ```

* Never return from an `ensure` block. If you explicitly return from a
  method inside an `ensure` block, the return will take precedence over
  any exception being raised, and the method will return as if no
  exception had been raised at all. In effect, the exception will be
  silently thrown away.

    ```ruby
    def foo
      begin
        fail
      ensure
        return 'very bad idea'
      end
    end
    ```

* Use *implicit begin blocks* when possible.

    ```ruby
    # bad
    def foo
      begin
        # main logic goes here
      rescue
        # failure handling goes here
      end
    end

    # good
    def foo
      # main logic goes here
    rescue
      # failure handling goes here
    end
    ```

* Mitigate the proliferation of `begin` blocks by using
  *contingency methods* (a term coined by Avdi Grimm).

    ```ruby
    # bad
    begin
      something_that_might_fail
    rescue IOError
      # handle IOError
    end

    begin
      something_else_that_might_fail
    rescue IOError
      # handle IOError
    end

    # good
    def with_io_error_handling
       yield
    rescue IOError
      # handle IOError
    end

    with_io_error_handling { something_that_might_fail }

    with_io_error_handling { something_else_that_might_fail }
    ```

* Don't suppress exceptions (let the team know when bad things happen).

    ```ruby
    # bad
    begin
      # an exception occurs here
    rescue SomeError
      # the rescue clause does absolutely nothing
    end

    # bad
    do_something rescue nil
    ```

* Don't use exceptions for flow control.

    ```ruby
    # bad
    begin
      n / d
    rescue ZeroDivisionError
      puts 'Cannot divide by 0!'
    end

    # good
    if d.zero?
      puts 'Cannot divide by 0!'
    else
      n / d
    end
    ```

* Avoid rescuing the `Exception` class.  This will trap signals and calls to
  `exit`, requiring you to `kill -9` the process.

    ```ruby
    # bad
    begin
      # calls to exit and kill signals will be caught (except kill -9)
      exit
    rescue Exception
      puts "you didn't really want to exit, right?"
      # exception handling
    end

    # good
    begin
      # a blind rescue rescues from StandardError, not Exception as many
      # programmers assume.
    rescue => e
      # exception handling
    end

    # also good
    begin
      # an exception occurs here

    rescue StandardError => e
      # exception handling
    end

    ```

* Put more specific exceptions higher up the rescue chain, otherwise
  they'll never be rescued from.

    ```ruby
    # bad
    begin
      # some code
    rescue Exception => e
      # some handling
    rescue StandardError => e
      # some handling
    end

    # good
    begin
      # some code
    rescue StandardError => e
      # some handling
    rescue Exception => e
      # some handling
    end
    ```

* Release external resources obtained by your program in an ensure block.

    ```ruby
    f = File.open('testfile')
    begin
      # .. process
    rescue
      # .. handle error
    ensure
      f.close unless f.nil?
    end
    ```

* Favor the use of exceptions for the standard library over
introducing new exception classes.

## Collections

* Prefer literal array and hash creation notation (unless you need to
pass parameters to their constructors, that is).

    ```ruby
    # bad
    arr = Array.new
    hash = Hash.new

    # good
    arr = []
    hash = {}
    ```

* Prefer `%w` to the literal array syntax when you need an array of strings.

    ```ruby
    # bad
    STATES = ['draft', 'open', 'closed']

    # good
    STATES = %w(draft open closed)
    ```

* Avoid the creation of huge gaps in arrays.

    ```ruby
    arr = []
    arr[100] = 1 # now you have an array with lots of nils
    ```

* Use `Set` instead of `Array` when dealing with unique elements. `Set`
  implements a collection of unordered values with no duplicates. This
  is a hybrid of `Array`'s intuitive inter-operation facilities and
  `Hash`'s fast lookup.
* Prefer symbols instead of strings as hash keys.

    ```ruby
    # bad
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

* Avoid the use of mutable objects as hash keys.
* The new hash literal syntax is preferred in Ruby 1.9 when your hash keys are symbols.

    ```ruby
    # bad
    hash = { :one => 1, :two => 2, :three => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

* Rely on the fact that hashes in Ruby 1.9 and above are ordered.
* Never modify a collection while traversing it.

## Strings

* Prefer string interpolation instead of string concatenation:

    ```ruby
    # bad
    email_with_name = user.name + ' <' + user.email + '>'

    # good
    email_with_name = "#{user.name} <#{user.email}>"
    ```

* Prefer single-quoted strings when you don't need string interpolation or
  special symbols such as `\t`, `\n`, `'`, etc.

    ```ruby
    # bad
    name = "Bozhidar"

    # good
    name = 'Bozhidar'
    ```

* Don't use `{}` around instance variables being interpolated into a
  string.

    ```ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      # bad
      def to_s
        "#{@first_name} #{@last_name}"
      end

      # good
      def to_s
        "#@first_name #@last_name"
      end
    end
    ```

* Avoid using `String#+` when you need to construct large data chunks.
  Instead, use `String#<<`. Concatenation mutates the string instance in-place
  and is always faster than `String#+`, which creates a bunch of new string objects.

    ```ruby
    # good and also fast
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

## Regular Expressions

* Don't use regular expressions if you just need plain text search in string:
  `string['text']`
* For simple constructions you can use regexp directly through string index.

    ```ruby
    match = string[/regexp/]             # get content of matched regexp
    first_group = string[/text(grp)/, 1] # get content of captured group
    string[/text (grp)/, 1] = 'replace'  # string => 'text replace'
    ```

* Use non-capturing groups when you don't use captured result of parentheses.

    ```ruby
    /(first|second)/   # bad
    /(?:first|second)/ # good
    ```

* Avoid using $1-9 as it can be hard to track what they contain. Named groups
  can be used instead.

    ```ruby
    # bad
    /(regexp)/ =~ string
    ...
    process $1

    # good
    /(?<meaningful_var>regexp)/ =~ string
    ...
    process meaningful_var
    ```

* Character classes have only few special characters you should care about:
  `^`, `-`, `\`, `]`, so don't escape `.` or brackets in `[]`.

* Be careful with `^` and `$` as they match start/end of line, not string endings.
  If you want to match the whole string use: `\A` and `\z` (not to be
  confused with `\Z` which is the equivalent of `/\n?\z/`).

    ```ruby
    string = "some injection\nusername"
    string[/^username$/]   # matches
    string[/\Ausername\z/] # don't match
    ```

* Use `x` modifier for complex regexps. This makes them more readable and you
  can add some useful comments. Just be careful as spaces are ignored.

    ```ruby
    regexp = %r{
      start         # some text
      \s            # white space char
      (group)       # first group
      (?:alt1|alt2) # some alternation
      end
    }x
    ```

* For complex replacements `sub`/`gsub` can be used with block or hash.

## Percent Literals

* Use `%w` and `%i` freely.

    ```ruby
    STATES = %w(draft open closed)
    ```

* Use `%()` for single-line strings which require both interpolation
  and embedded double-quotes. For multi-line strings, prefer heredocs.

    ```ruby
    # bad (no interpolation needed)
    %(<div class="text">Some text</div>)
    # should be '<div class="text">Some text</div>'

    # bad (no double-quotes)
    %(This is #{quality} style)
    # should be "This is #{quality} style"

    # bad (multiple lines)
    %(<div>\n<span class="big">#{exclamation}</span>\n</div>)
    # should be a heredoc.

    # good (requires interpolation, has quotes, single line)
    %(<tr><td class="name">#{name}</td>)
    ```

* Use `%r` only for regular expressions matching *more than* one '/' character.

    ```ruby
    # bad
    %r(\s+)

    # still bad
    %r(^/(.*)$)
    # should be /^\/(.*)$/

    # good
    %r(^/blog/2011/(.*)$)
    ```

* Avoid `%q`, `%Q`, `%x`, `%s`, and `%W`.

* Prefer `()` as delimiters for all `%` literals.

## Kernel methods

* Use Ruby Kernel methods for coercion instead of checking for `nil?` or `empty?`.

    ```ruby
    # bad
    if results_array
      results_array
    else
      []
    end

    # good
    Array(results_array)

    # bad
    if some_string.empty?
      ''
    else
      some_string
    end

    # good
    String(some_string)
    ```

## Metaprogramming

* Avoid needless metaprogramming.

* Do not mess around in core classes when writing libraries. (Do not monkey patch them.)

* Prefer the usage of [refinements](http://ruby-doc.org/core-2.1.1/doc/syntax/refinements_rdoc.html)
  when adding behavior to core classes.

* The block form of `class_eval` is preferable to the string-interpolated form.
  - when you use the string-interpolated form, always supply `__FILE__` and `__LINE__`, so that your backtraces make sense:

    ```ruby
    class_eval 'def use_relative_model_naming?; true; end', __FILE__, __LINE__
    ```

  - `define_method` is preferable to `class_eval { def ... }`

* When using `class_eval` (or other `eval`) with string interpolation,
  add a comment block showing its appearance if interpolated (a practice learned from the Rails code):

    ```ruby
    # from activesupport/lib/active_support/core_ext/string/output_safety.rb
    UNSAFE_STRING_METHODS.each do |unsafe_method|
      if 'String'.respond_to?(unsafe_method)
        class_eval <<-EOT, __FILE__, __LINE__ + 1
          def #{unsafe_method}(*args, &block)       # def capitalize(*args, &block)
            to_str.#{unsafe_method}(*args, &block)  #   to_str.capitalize(*args, &block)
          end                                       # end

          def #{unsafe_method}!(*args)              # def capitalize!(*args)
            @dirty = true                           #   @dirty = true
            super                                   #   super
          end                                       # end
        EOT
      end
    end
    ```

* Avoid using `method_missing` for metaprogramming. Some problems are:
  - Backtraces become messy;
  - the behavior is not listed in `#methods`;
  - misspelled method calls might silently work (`nukes.launch_state = false`).

* Consider using delegation, proxy, or `define_method` instead of `method_missing`.

* If `method_missing` is your **ONLY OPTION**:
  - be sure to [also define `respond_to_missing?`](http://blog.marc-andre.ca/2010/11/methodmissing-politely.html)
  - only catch methods with a well-defined prefix, such as `find_by_*` -- make your code as assertive as possible.
  - call `super` at the end of your statement.
  - delegate to assertive, non-magical methods:

    ```ruby
    # bad
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        # ... lots of code to do a find_by
      else
        super
      end
    end

    # good
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        find_by(prop, *args, &block)
      else
        super
      end
    end

    # Best of all, though, would to use `define_method` as each findable attribute is declared
    ```

## Misc

* Avoid methods longer than 10 LOC (lines of code). Ideally, most methods will be shorter than
  5 LOC. Empty lines do not contribute to the relevant LOC.
* Avoid parameter lists longer than three or four parameters.
* If you really have to, add "global" methods to Kernel and make them private.
* Use class instance variables instead of global variables.

    ```ruby
    # bad
    $foo_bar = 1

    # good
    class Foo
      class << self
        attr_accessor :bar
      end
    end

    Foo.bar = 1
    ```

* Avoid `alias` when `alias_method` will do.
* Use `OptionParser` for parsing complex command line options and
`ruby -s` for trivial command line options.
* Code in a functional way, avoiding mutation when that makes sense.
* Do not mutate arguments unless that is the purpose of the method.
* Avoid more than three levels of block nesting.
* Be consistent. In an ideal world, be consistent with these guidelines.
* Use common sense.

# Contributing

Nothing written in this guide is set in stone. This is used as a guideline for
internal and open source projects at Magnetis. But suggestions are welcome!

Feel free to open tickets or send pull requests with improvements. Thanks in
advance for your help!

**[⬅ back to main](../../)**&nbsp;&nbsp;&nbsp;&nbsp;**[⬆ back to top](#table-of-contents)**
