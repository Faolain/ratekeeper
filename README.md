# Ratekeeper

Ratekeeper is a library for scheduling rate-limited actions.
It supports complex rate limits and estimates time left to resetting limits.


## Installation

Add `ratekeeper` as dependency in `mix.exs`

``` elixir
def deps do
    [{:ratekeeper, "~> 0.2"}]
end
```

## Usage

Limits can be set in config:
```elixir
config :ratekeeper, :limits, %{"myapi.org" => [{1000, 5}, {60_000, 100}]}
```
or at runtime:
```elixir
Ratekeeper.add_limit "myapi.org", 1000, 5
Ratekeeper.add_limit "myapi.org", 60_000, 100
```
This sets limits to *5 requests per 1 second* and *100 requests per minute*.

Appoint a request to rate limited api:
```elixir
case Ratekeeper.register("myapi.org", 10_000) do
  nil ->
    raise "Rate limits exceeded, request not allowed in next 10 seconds"
  delay ->
    :timer.sleep(delay)
    MyApi.do_request()
end
```

Full documentation can be found at [https://hexdocs.pm/ratekeeper](https://hexdocs.pm/ratekeeper).

