+++
Categories = ["documentation"]
Description = ""
Tags = ["tools"]
date = "2015-10-24T12:12:11-07:00"
title = "Browse the Go Documentation Without an Internet Connection"
+++

If you travel a lot (or if you live in the boondocks, like I do), then you'll probably find yourself in a situation where you need to browse the documentation for the packages in the Go standard library, but the internet connection is terrible or just plain non-existent.

To get around this, run a godoc server locally on an available port. I've often seen people use 6060 or 4040.

    godoc -http=":6060"

This gives you access to all the installed packages at [localhost:6060/pkg][1]: the Go standard library, as well as any other packages you may have installed.

[1]: http://localhost:6060/pkg
