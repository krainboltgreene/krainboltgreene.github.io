One day I was really frustrated the ruby package `rack`, so I imagined what a better rack would look like.

``` ruby
require "webstack"
require "webstack-throttle"
require "webstack-protection"
require "webstack-tempfile_manager"
require "webstack-deserializer"
require "webstack-dispatcher"
require "webstack-content_length_setter"
require "webstack-content_type_setter"
require "webstack-accept_setter"
require "webstack-serializer"

# Each cycle calls this server block.
# The request and response objects are semi-mutable, see: webstack2.rb.
Webstack::Server.new do |stack, request, response|
  Throttle::Middleware.new(request)
  Protection::Middleware.new(request)
  TempfileManager::Middleware.new(request)
  Deserializer::Middleware.new(request)

  Dispatcher.new(stack)

  ContentLengthSetter::Middleware.new(response)
  ContentTypeSetter::Middleware.new(response)
  AcceptSetter::Middleware.new(response)
  Serializer::Middleware.new(response)
end
```

Each middleware piece is handed the appropriate part, allowed to mutate
sub-sections, and then is garbage collected.
If a middleware encounters a problem it can do one of two things:
  - Halt the stack with a status code (4XX, 5XX), see: throttle & protection
  - Add an error body instead of a mutated raw, see: deserializer & tempfile_manager
This allowes the stack designer to handle errors in the desired
way.

Pros:

  - Each middleware is given a clear designation on the cycle
  - Raw data isn't mutated, ever, allowing access to known untainted information
  - The stack is clean and clear cut, defining boundries for middleware
  - The middleware is capable of being immutable
  - Error handling is no longer up to the middleware developer, no more
    json error responses in your XML endpoint
  - Middleware can now share behavior
  - It's efficient to debug the process, as things happen once and get
    memoized.
  - Middleware can't fuck with other middleware.

Cons:

  - No more `#call` affinity, so no more Procs or Lambdas.
  - Might be bigger in memory with the multiple states, keys, etc.


``` ruby
# The content of response

{
  # raw is immutable
  "raw" => {
    "request" => "GET /accounts?limit=10",
    "header" => {
      "Content-Type" => "application/json",
      "Accept" => "application/json"
    },
    "body" => #<StringIO#f2edasd>
  },
  # modified is writable but no overwrites or deletes
  "modified" => {
    "webstack" => {
      "verb" => :get,
      "path" => "/accounts",
      "query" => {
        "limit" => 10
      },
      "header" => {
        "Content-Type" => "application/json",
        "Accept" => "application/json"
      }
      "body" => #<StringIO#f2edasd>
    },
    "throttle::middleware" => {
      # ... throttles mutated version of raw ...
    }
    # ... Other middleware's versions of raw ...
  }
}

# At the end all of modified's headers & query are merged and last body, verb, path, status are used.


# The content of request

{
  "webstack" => {
    "status" => 200,
    "header" => {},
    "body" => nil
  },
  "content_length_setter::middleware" => {
    "header" => {
      "Content-Length" => 300
    },
    "body" => nil
  }
  # ... Other middleware's structs ...
}

# At the end all headers are merged and last body & status are used.
```