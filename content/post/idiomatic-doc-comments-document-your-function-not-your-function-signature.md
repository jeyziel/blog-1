+++
Categories = ["documentation"]
Description = ""
Tags = ["idioms"]
date = "2015-10-14T12:07:58-07:00"
title = "Idiomatic Doc Comments: Document Your Function, Not Your Function Signature"
+++

It's quite possible to run `golint` without complaint and end up with documentation that is mediocre and unhelpful.

Part of the problem is that programmers coming to Go from other languages have a lot of baggage.

If you took Java classes in college, there's a chance that you've been taught to document all your inputs and outputs for every public method. The comments end up being oh-so machine-readable, and oh-so tedious to parse with your eyes.

If you've been working in dynamic languages for a while, then chances are great that you've embraced the philosophy that comments are, at the very best, noise, and at the worst, lies. You might argue that comments are a form of duplication and that code should tell such a good story that comments become superfluous.

If your career has been focused on shipping code in fast-paced environments, then you've likely got a minimalistic and very pragmatic approach to documentation. Helping colleagues (or future you) avoid going down that rabbit hole (again!) is considered essential, but for the most part, commenting code seems like an academic exercise, not a practical concern in the real world.

If you're not a programmer yet and are working your way up to your first programming job, then it's even harder. You're stuck with the blank slate. It can be difficult to imagine what information could possibly be useful to someone later.

Here's a comment that makes `golint` happy, but not much else.

    // IsLeapYear defines a function IsLeapYear(int) bool.
    func IsLeapYear(year int) bool {
      // ...
    }

It starts with the function name, and it's a full, natural-language sentence, just as the [commentary section of Effective Go][1] recommends.

But take a look at how this reads when looking at the output of `go doc`.

    go doc cal.IsLeapYear
    func IsLeapYear(year int) bool
        IsLeapYear defines a function IsLeapYear(int) bool.

It practically echoes.

The various documentation tools in Go, [go doc, godoc, and godoc.org][2] all display the function signature along with the doc comments.

A good comment is about the _function_. A great comment will answer questions someone might have when browsing the documentation. What does the function do? Why would you want to use it? What problem does it solve? What are the gotchas?

&gt; Godoc comments are just good comments, the sort you would want to read even if godoc didn't exist.  
  
– [The Go Blog][3]  

The standard library is full of great examples.

    go doc os.Exit
    func Exit(code int)
        Exit causes the current program to exit with the given status code.
        Conventionally, code zero indicates success, non-zero an error. The program
        terminates immediately; deferred functions are not run.

This comment not only tells us what the function does (_causes the current program to exit with the given status code_), but also provides information about conventions (_code zero indicates success_), and also important and non-obvious information (_deferred functions are not run_).

A better comment for the leap year function would note that this is valid for the Gregorian calendar. It might also mention that it's valid for years that occurred before the Gregorian reform in 1582, but that it's not valid for negative years (representing years numbered with BC notation).

It might even link to the delightful [4 minute video][4] by CGP Gray that explains why we even have leap years.

[1]: https://golang.org/doc/effective_go.html#commentary
[2]: http://whipperstacker.com/2015/09/30/go-documentation-godoc-godoc-godoc-org-and-go-doc/
[3]: http://blog.golang.org/godoc-documenting-go-code
[4]: https://www.youtube.com/watch?v=xX96xng7sAE
