# Motion::HTTP (WIP)

Cross-platform HTTP library for RubyMotion.

Delegates to the best networking library on each platform, and gives you an escape hatch when you need it.

## Usage

Motion::HTTP is built on moving around `Motion::HTTP::Request` objects.

### Building Requests

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

### Running Requests

```ruby
Motion::HTTP.get/post/put/delete/head(request) do |response|
end
```

### Parsing Responses

```ruby
response.status_code
# => 200
response.headers["Content-Type"]
# => "application/json"
```

### Clients

If you want to share configuration/headers/etc for groups of requests, use `Motion::HTTP::Client`:

```ruby
client = Motion::HTTP::Client.new
client.base_url = "http://api.com"
client.headers["Accept"] = "application/json"

client.get(request) do |result|
end
```
