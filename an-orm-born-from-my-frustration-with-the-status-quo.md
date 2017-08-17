The idea behind this ORM is it mimics Ember.js's object existance model. That is, when Ember.js expects an object to exist and it doesn't it will provide a skeleton version with the basic needed functionality.

The idea is that you start off with a class that receives the model behavior set. When and if it needs a: serializer, normalizer, sanitizer, validator, or deserializer it can come up with one on the fly if the environment hasn't defined one. We do this by taking the scope (`Person`) and looking for constants in that scope that match the desired naming scheme: For instance when looking for a `Sanitizer` we look for `Person::Santizer`. When looking to valiate the Name field we look for `Person::Validator::NameValidator`.

I believe this is powerful for two reasons:

  1. The developer can control, to a fine grain, various business logic about his models. For instance, he can allow most of his models to store to one database, but if needed he can define `Person::Persistance` with an enirely different persistance logic (some cloud database?).
  2. It follows the Principle of Least Work, in that no effort can still return a working solution.

I've taken these cues from both Ember.js and a few blog posts I've read over the years about progressive utility (like Progressive Enhancement, but instead of allowing for more responsive behavior if possible, you allow for more fine-grained utility).

``` ruby
class Person
  include ORM::Model

  # Take data from the outside world and parse it
  # Without definition of any nature this is an empy
  # class and will expect native types
  class Deserializer
    def inititalize(model, attributes)
      @model = model
      @attributes = attributes
    end

    def native
      Hash.try_convert(@attributes)
    end
  end

  # Fit the outside data structure to match the
  # internal data structure. This will only grab
  # values defined by the model via the `accessible`
  # method if not personally defined.
  class Normalizer
    def initialize(model, attributes, context = "default")
      @model = model
      @attributes = attributes
      @context = context
    end

    def normal
      @model.accessibles.fetch(@context).inject({}) do |normal, accessible|
        normal.update(accessible, @native.fetch(accessible))
      end
    end
  end

  # Cleans up the values of the incoming data. If
  # given no definition it will simply attempt to
  # coerce simplistic values based on the
  # Persistance object, like Integer, String, Array,
  # etc.
  class Sanitizer
    def initialize(model, attributes)
      @model = model
      @attributes = attributes
    end

    def clean
      @attributes.inject({}) do |attributes, attribute|
        # WRITE THIS
      end
    end
  end

  # Checks the validity of the incoming data. Without
  # definition this class will only check for nil
  # values being passed to null-not-allowed fields
  # on the persistance object
  class Validator
    def initialize(model, attributes)
      @model = model
      @attributes = attributes
    end

    def validate

    end
  end

  # Stores the data
  class Storer
    def initialize(model, attributes)
      @model = model
      @attributes = attributes
    end

    def store

    end
  end

  class Retriever
    def initialize(model, attributes)
      @model = model
      @attributes = attributes
    end

    def retrieve

    end
  end

  # Turns the record into common formats for output
  class Serializer
    def initialize(model, attributes)
      @model = model
      @attributes = attributes
    end

    def to_json

    end
  end
end
```

``` ruby
# Simple example
class Person
  include ORM::Model

  # Persistance details are the only "required" part of the short way
  field :name, index: true
  field :age, Integer
  field :description, ORM::SQL::Text, unique: true

  many :pets, Pet
  one :car, Car
  reference :company, Company

  # Want a generic serialization?
  to :json do
    attribute :href
    attribute :id
    attribute :first_name
    attribute :last_name
  end

  # Want a generic validation?
  validate :name, uniqueness: true
  validate :age, Validator::Age
  validate :description do |model, value|
    value.truthy?
  end
end
```