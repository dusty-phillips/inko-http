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
inko pkg add github.com/dusty-phillips/inko-http.git 0.0.1
inko pkg sync
```

Create `src/main.inko` with something like this:

```

```
