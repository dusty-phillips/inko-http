# Inko Http Server

This is a (very) rudimentary HTTP 1.1 server I wrote to explore the [Inko](https://inko-lang.org)
programming language's concurrency and package management features.

It does very little, and it does it very poorly. It does seem to compile and run, though, and
I guess that's something.

## Getting Started

I developed this against Inko's main branch. It might work with Inko version 0.11 which was recently
released, but there are a couple compiler bugs that have been fixed since 0.11 was released, so
I recommend using main or something greater than 0.11.0.

Follow the Inko [installation instructions].

Create a new project and add `http` as a dependency:

```
mkdir my-inko-webapp
cd my-inko-webapp
mkdir src
inko pkg init
inko pkg add github.com/dusty-phillips/inko-http.git 0.0.2
inko pkg sync
```

Create `src/main.inko` with something like this:

```
import std::net::ip
import http::app_request::(AppRequest)
import http::handler::(Handler)
import http::http_response::(HttpResponse, HttpStatus)
import http::route::(FixedRoute)
import http::uni::(Uni)
import http::webapp::(WebApp)

class HelloHandler {}

impl Uni[HelloHandler] for HelloHandler {
  fn pub clone_to_uni -> uni HelloHandler {
    recover HelloHandler {}
  }

  fn pub move into_referrable -> HelloHandler {
      self
  }
}


impl Handler for HelloHandler {
  fn pub handle(request: ref AppRequest) -> HttpResponse {
    let response = HttpResponse.new(HttpStatus.OK)
    response.body = "Hello World"
    response
  }
}

class async Main {
  fn async main {
      let app = WebApp.new("0.0.0.0", 2023).unwrap
      app.add_route(recover FixedRoute.get("/hello"), recover HelloHandler {})
      app.listen()
  }
}
```

(Yes, I have ideas to make this less verbose. No I don't have plans to implement them.)

Now you can use `inko run` and look at http://localhost:2032/hello in your browser.
For good measure, check that http://localhost:2032/goodbye returns a 404 NOT FOUND.

You can also create handlers for routes with named parameters:

```
class NameHandler {}

impl Uni[NameHandler] for NameHandler {
  fn pub clone_to_uni -> uni NameHandler {
    recover NameHandler{}
  }

  fn pub move into_referrable -> NameHandler {
    self
  }
}

impl Handler for NameHandler {
  fn pub handle(request: ref AppRequest) -> HttpResponse {
    let response = HttpResponse.new(HttpStatus.OK)
    response.body = "Hello {request.path_params.get('name')}"
    response
  }
}
```

Then update your main function to add the param route:

```
    app.add_route(recover ParamRoute.get('/hello/{name}'), recover NameHandler {})
```

Which will require adding `ParamRoute` to the existing `http::route` import at the top of the file.

That's about all it does right now. I cut corners all over the code and it doesn't come near fully
implementing the Http 1.1 RFC (but I did read the whole thing. It's quite interesting.)

So... don't use this for production traffic. 😛
