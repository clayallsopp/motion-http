# Motion::HTTP (WIP)

Cross-platform HTTP library for RubyMotion.

Delegates to the best networking library on each platform (AFNetworking on iOS, OkHttp on Android), and gives you an escape hatch when you need it.

## Usage

### Building Requests

Motion::HTTP is structured around around `Motion::HTTP::Request` objects.

```ruby
request = Motion::HTTP::Request.new

request.method = :get
request.url = "http://google.com"
# add parameters
request.parameters[:q] = "RubyMotion"
# add a multipart parameter
request.parameters[:image] = Motion::HTTP::MultipartParameter.new(file)
# add headers
request.headers["Accept"] = "application/json"
```

You can also initialize a `Request` with a `Hash`:

```ruby
request = Motion::HTTP::Request.new(
  method: :get,
  url: "http://google.com",
  parameters: {q: "RubyMotion"},
  headers: {"Accept" => "application/json"}
)
```

There is also an alternate syntax:

```ruby
# todo: make this better? or remove it?
request = Motion::HTTP::Request.get("http://google.com",
  q: "RubyMotion",
  Motion::HTTP::HEADERS => {"Accept" => "application/json"}
)
```

### Running Requests

Motion::HTTP uses reasonable defaults for running requests:

- All requests are run asynchronously on non-UI threads
- Responses return on the UI thread

```ruby
Motion::HTTP.run(request_or_constructor) do |response|
end

Motion::HTTP.get/post/put/delete/head(request_or_constructor) do |response|
end
```

### Parsing Responses

A `Motion::HTTP::Response` is returned in the callback

TODO: how does success/failure work? chaining/filters?

```ruby
response.status_code
# => 200
response.headers["Content-Type"]
# => "application/json"
response.body
# => '{"error": "thing happened"}'
response.native_response
# => #<AFHTTPRequestOperation>
```

### Clients

If you want to share configuration/headers/etc for groups of requests, use `Motion::HTTP::Client`. Any configuration in a `Motion::HTTP::Request` instance will override the client's configuration:

```ruby
client = Motion::HTTP::Client.new
client.base_url = "http://api.com"
client.headers["Accept"] = "application/json"

client.get(request) do |result|
end
```
