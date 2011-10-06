Http
====

Ruby has always been this extremely web-focused language, and yet despite the
selection of HTTP libraries out there, I always find myself falling back on
Net::HTTP, and Net::HTTP sucks.

Ruby should be simple and elegant and beautiful. Net::HTTP is not. I've often
found myself falling back on the Perlish horrors of open-uri just because I
found Net::HTTP to be too much of a pain. This shouldn't be!

HTTP should be simple and easy! It should be so straightforward it makes
you happy with how delightful it is to use!

Making Requests
---------------

Let's start with getting things:

    Http.get "http://www.google.com"

That's it! The result is the response body.

Don't like "Http"? No worries, this works as well:

    HTTP.get "http://www.google.com"

After all, There Is More Than One Way To Do It!

Adding Headers
--------------

The Http library uses the concept of chaining to simplify requests. Let's say
you want to get the latest commit of this library from Github in JSON format.
One way we could do this is by tacking a filename on the end of the URL:

    Http.get "https://github.com/tarcieri/http/commit/HEAD.json"

The Github API happens to support this approach, but really this is a bit of a
hack that makes it easy for people using browsers to perform the act of
content negotiation. HTTP would really prefer we do this using the Accept
header. How do we send that with the Http library?

    Http.with_headers(:accept => 'application/json').
      get("https://github.com/tarcieri/http/commit/HEAD")

This requests JSON from Github. Github is smart enough to understand our
request and returns a response with Content-Type: application/json. If you
happen to have a library loaded which defines the JSON constant and implements
JSON.parse, the Http library will attempt to parse the JSON response.

A shorter alias exists for HTTP.with_headers:

	Http.with(:accept => 'application/json').
	  get("https://github.com/tarcieri/http/commit/HEAD")