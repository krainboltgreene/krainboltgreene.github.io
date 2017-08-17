> For reference I wrote this article about a year ago according to the commits at the `astruct` [repo](https://github.com/krainboltgreene/astruct). I never submitted it because I got frustrated with the Ruby MRI source contribution process. `astruct` could definitely use some cleaning up, but actually runs much better in 2.1.0 so the details still stand.

Hello Ruby Core,

In this Pull Request I have made a series of changes that I would like to be added to the Ruby stdlib library `ostruct`. Before getting into anything I want to stress one thing: **All changes are backwards compatible with the previous ostruct library**. Any non-compatible changes are what I consider bug fixes and assumed behavior corrections.

First a list of problems with the current OpenStruct class:

  * `OpenStruct` is used to create open structured objects and also is often used to give it's open structure behavior to other classes. It is not always desirable or favorable to force the developer to inherit this class.
  * `OpenStruct` behaves somewhat similarly to a `Hash`, but lacks a full duck typing and assumed behavior. Importantly to this point `OpenStruct` can be built from, updated with, and dump to a `Hash`. It even has a similar public interfaces like `Hash#delete()`, but defined as `OpenStruct#delete_field()`, or `merge` and `merge!` as `OpenStruct#marshal_load()`.
  * All of the public and private methods on the `OpenStruct` are open to easy and annoying overloading. Some of the current methods use other public methods that are *likely* to be overloaded. A good example of this danger is the current `OpenStruct#inspect` method, which calls `OpenStruct#object_id`. It also causes problems like this:

    ``` ruby
    example = OpenStruct.new
    example.marshal_load = "example;example;example"
      # => "example;example;example"
    example.table
      # => nil
    example
      # => #<OpenStruct marshal_load="example;example;example">
    example.marshal_load
      # => ArgumentError: wrong number of arguments (0 for 1)
    example.marshal_load "red"
      # => NoMethodError: undefined method `each_key' for "red":String
    example
      # => #<OpenStruct:0x3fd970c44120>
    example.table
      # => TypeError: can't convert Symbol into Integer
    ```

Second, in order to address the issues I was having I first built [a gem](http://rubygems.org/gems/astruct) to solve my problem. All of the below things I've done have been done in this gem and remain readily available for your use.

  * The `OpenStruct#delete_field` method only deletes the table key/value. This is not the expected behavior and ultimately leaves a lot of junk around in the method table.
  * When using `OpenStruct#marshal_load` it would correctly create getter and setter methods for each key/value, but it would also delete the entire table. I don't believe this to be expected or intelligent behavior.
  * One of the tests freezes an `OpenStruct` object and then redefines `OpenStruct#frozen?` and it works. Freezing should stop any changes to the object.
  * In order to allow developers to create both `OpenStruct`-like objects and create regular `OpenStruct` objects original I have created a module nested in the `OpenStruct` class called `Behavior`. That module contains all of the behavior `OpenStruct` actually receives. Here's an example of what this affords:

    ``` ruby
    class Configuration < BasicObject
      include OpenStruct::Behavior
    end
    ```

  * I've designed it so that all the public methods are wrapped in double underscores in the same way that `Object#__send__` and many other important methods are named.
  * I've aliased "underscore" methods to the important methods like `OpenStruct#__freeze__` to `OpenStruct#freeze`.
  * In addition, I have aliased many methods to their "underscore" forms (ie. `OpenStruct#inspect` aliased to `OpenStruct#__inspect__`) to avoid confusion. This works even when someone later overloads those method names.
  * Because of the nature of `alias_method` you can now define a key/value "object\_id" and still have access to the original `OpenStruct#object_id` with `OpenStruct#__object_id__`. This avoids the overloading bug explained previously.
  * I have added the ability to only dump a specific set of keys with the `OpenStruct#__dump__` method. This was added to further `Hash` like behavior.
  * I have further improved the tests for this library by either breaking them up into manageable pieces or refined for readability. There are now test for other features of `OpenStruct` as well as tests for the new features added in this pull request.

In addition to fixing bugs, improving the interface and removing surprising behavior, I used two types of benchmarking: `Benchmark#bmbm` and `Benchmark#ips` (`benchmark-ips` gem), both showing favorable results for my version of ostruct. Here are [the results](https://gist.github.com/3462357)