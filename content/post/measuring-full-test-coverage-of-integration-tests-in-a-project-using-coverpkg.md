+++
Categories = ["testing"]
Description = ""
Tags = ["tools"]
date = "2015-10-01T11:08:50-07:00"
title = "Measuring Full Test Coverage of Integration Tests in a Project using -coverpkg"
+++


When you have integration tests that are wiring a bunch of things together, it's not uncommon to get suspiciously low test coverage scores when using `go test -cover`. You'll often see this with tests for REST API handlers, or command-line interface (CLI) commands.

I've even seen scores of 0%, when clearly there is code being exercised. It's just that the code happens to live in other packages within the project.

Because these tests tie so many things together, what you really want to know is much of the _entire_ codebase is exercised when they are run.

To illustrate, [fish][1] is a small, somewhat whimsical command-line application, that provides random fish-ascii art.

It takes a `-msg` flag, and outputs a fish that swims and talks.

    go run main.go -msg="Lovely day for a swim" ./...

              o˚• Lovely day for a swim .o.˚
    ><«««º> ·°

In addition to main, there are two packages: `bubbles` and `fish`.

    .
    ├── bubbles
    │   └── bubbles.go
    ├── fish
    │   └── fish.go
    ├── main.go
    └── main_test.go

The tests for the application live in `main_test.go`, and they're very high level. The tests require the `fish` package, and the fish package, in turn, requires the `bubbles` package.

## Measuring Test Coverage with -cover

Running with -cover gives 33% coverage.

    go test -cover
    PASS
    coverage: 33.3% of statements
    ok  	github.com/kytrinyx/fish	0.021s

That seems low. We can get some statistics about exactly what was exercised by the tests by using the `-coverprofile` flag.

    go test -coverprofile=cover.out
    PASS
    coverage: 33.3% of statements
    ok  	github.com/kytrinyx/fish	0.024s

Using the go tool we can take a closer look at those stats.

    go tool cover -func=cover.out
    github.com/kytrinyx/fish/main.go:16:	main		0.0%
    github.com/kytrinyx/fish/main.go:23:	Doodle		100.0%
    total:					(statements)	33.3%

Main is essentially a one-liner, and doesn't need to be tested. Doodle is also a one-liner, so 100% test coverage makes sense. But Doodle calls fish, which in turn calls bubbles, and we're not seeing any stats for either of those packages.

We need to get the stats for the dependencies.

## Measuring Test Coverage with -coverpkg

The flag that does this is `-coverpkg`. It takes a comma-separated list of the packages you're interested in, and it also accepts the handy `./...` shortcut that so many other go commands understands to mean _this package and all packages in the subdirectories, recursively_.

    go test -coverpkg=./...
    PASS
    coverage: 81.0% of statements in ./...
    ok  	github.com/kytrinyx/fish	0.021s

81% is excellent test coverage. Let's see what's included.

    go test -coverpkg=./... -coverprofile=cover.out
    go tool cover -func=cover.out

    github.com/kytrinyx/fish/bubbles/bubbles.go:21:	New		66.7%
    github.com/kytrinyx/fish/bubbles/bubbles.go:29:	Embellish	100.0%
    github.com/kytrinyx/fish/bubbles/bubbles.go:34:	Blub		100.0%
    github.com/kytrinyx/fish/bubbles/bubbles.go:42:	randomBubble	100.0%
    github.com/kytrinyx/fish/fish/fish.go:23:	randomBody	100.0%
    github.com/kytrinyx/fish/fish/fish.go:27:	randomEye	100.0%
    github.com/kytrinyx/fish/fish/fish.go:31:	randomFin	100.0%
    github.com/kytrinyx/fish/fish/fish.go:36:	New		66.7%
    github.com/kytrinyx/fish/fish/fish.go:47:	Say		100.0%
    github.com/kytrinyx/fish/main.go:16:		main		0.0%
    github.com/kytrinyx/fish/main.go:23:		Doodle		100.0%
    total:						(statements)	81.0%

It turns out the only things that are not covered are main (0%), bubbles.New (67%), and fish.New (67%).

That seems perfectly acceptable.

[1]: https://github.com/kytrinyx/fish

