+++
Categories = ["documentation"]
Description = ""
Tags = ["tools"]
date = "2015-09-30T10:59:12-07:00"
slug = "go-documentation-godoc-godoc-godoc-org-and-go-doc"
title = "Go Documentation: godoc, go doc, godoc.org, and go/doc—Which One’s Which?"
+++

Go takes its documentation [very seriously][1], and nobody would blame you if you found it confusing that someone might reference `godoc` in one breath and `go doc` in the next.

This isn't just a _to-may-to to-mah-to_ kind of thing. All of these concepts are related.

## godoc—the web server

The `godoc` tool is intended primarily to run a documentation server in your browser on [localhost][2].

    godoc -http=":6060"

It includes the documentation for the Go standard library, along with any package that lives in your GOPATH.

It can also format documentation as plain text on the command-line, where you would pass the import path of the package you are interested in. This was added as an afterthought.

    godoc golang.org/x/tools/godoc

Running `godoc` without any arguments will output the usage statement for the tool.

    usage: godoc package [name ...]
           godoc -http=:6060

    ...

## go doc—the go subcommand

The familiar `go` command-line tool, which is used to `go run` or `go build` or `go test`, also has a subcommand `doc`.

The `doc` subcommand will print out the documentation for whatever argument you pass it: a package, const, func, type, var, or method.

Unlike the `godoc` command, `go doc` was intended from the start to be convenient from the command-line.

The most important difference between `godoc` and `go doc` on the command-line, is that the syntax for `go doc` is, well, Go.

    go doc gob.Decoder

The equivalent command using `godoc` is a bit more awkward, requiring the full import path:

    godoc encoding/gob Decoder

The default behavior of `go doc` when calling it without arguments, is to display the documentation for the current package.

## godoc.org—the website

godoc.org is, confusingly, sometimes referred to as GoDoc. It hosts documentation for Go packages that are not a part of the Go standard library. Anything hosted publicly on Bitbucket, GitHub, Google Project Hosting or Launchpad is likely to show up on GoDoc. In fact, searching for a project by its import path will automatically add it to the site.

The documentation that you will see on godoc.org is very similar to the documentation that you get when running the local `godoc` server, but as pointed out in the comments, godoc.org doesn't use the output from the `godoc` tool.

## go/doc—the package

The command `godoc` and the website godoc.org both rely on the package `go/doc` to extract source code documentation.

If you were to use `godoc` to ask about `go/doc`, it would tell you all about it.

    $ godoc go/doc
    PACKAGE DOCUMENTATION

    package doc
        import "go/doc"

        Package doc extracts source code documentation from a Go AST.

    ...

## If in doubt…

When people are referring to the package go/doc in casual conversation, they'll often drop the slash. And when saying _go doc_ they could be referring to GoDoc, the website. And, you know how it is… they might actually be talking about the godoc tool.

If you're unsure of exactly which one they mean, ask for clarification!

## Summary

The package `go/doc` is used by other tools to extract the documentation comments from the AST.

The command-line tool `godoc` is a web server first, and outputs plain text documentation to STDOUT as an afterthought, whereas `go doc` is intended to be used to output documentation on the command-line.

The godoc.org website is the publicly searchable documentation for go packages that are not a part of the standard library.

_With thanks to Rob Pike for clarifying the reasoning behind the various tools._

[1]: https://golang.org/doc/effective_go.html#commentary
[2]: http://localhost:6060/pkg/
