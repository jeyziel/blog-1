+++
Categories = ["documentation"]
Description = ""
Tags = ["tools"]
date = "2015-10-03T11:52:34-07:00"
title = "Run a `godoc` Server Locally to See Documentation for Unpublished Packages"
+++

Gophers care a lot about documentation and people appreciate it immensely when you have a great overview, succinct summaries, great examples, and helpful details about gotchas.

Often it's only after publishing a library and then perusing the generated documentation on [godoc.org][1] that you start seeing the missing bits and pieces. Typos jump out. Awkward phrases make you want to cringe. You realize that you really need a good example for that one core function.

The publish–review–fix–republish cycle can be pretty tedious, and it makes for a noisy commit log.

A great way to short-circuit this cycle is to run a godoc server locally so that you can click around the documentation for your project in your own browser.

Pick a port to run the server on, say 6060, and start it with the following command:

    godoc -http=":6060"

Then you can browse all the installed packages at [localhost:6060/pkg][2]. You don't have to restart the server to get the changes when you edit a doc comment, just save the file and refresh the page in the browser.

For more about documenting your projects, check out the article [Godoc: Documenting Go Code][3] by Andrew Gerrand on the official Go blog.

[1]: https://godoc.org/
[2]: http://localhost:6060/pkg
[3]: http://blog.golang.org/godoc-documenting-go-code
