+++
Categories = []
Description = ""
Tags = ["stdlib", "cli"]
date = "2015-10-02T11:49:41-07:00"
title = "Customizing the Usage String for your Command-Line Tool with the `flag` Package"
+++

The default usage message that ships with the `flag` package in Go's standard library is very simple, listing the name of the program along with all the flags that are defined, ordering the flags lexicographically.

While in most cases simple is exactly what you need, sometimes your CLI needs a bit of personal flair.

The usage function on the `flag` package is defined as an exported variable, which allows you to override it.

There are two simple ways to override the variable:

1. redefine the flag.Usage inline
2. point flag.Usage to a custom function

## Redefining flag.Usage Inline

You can override the Usage variable by defining an unnamed function and assigning it to `flag.Usage`.

    func main() {
    	flag.Usage = func() {
    		// ...
    	}
    	// ...
    }

## Pointing flag.Usage to a Custom Function

If the custom definition is long or complex, you may want to define it as a separate function so you don't clutter up your `main`. You can then point `flag.Usage` to the custom function.

    func main() {
    	flag.Usage = usage

    	// ...
    }

    func usage() {
    	// ...
    }

## Refer to the Documentation

To see how the original function is defined, take a look at the output of `go doc`.

If you just want to change the header, there's not much to it. If you also want to change how the flags are displayed, then take a look at the documentation for `flag.PrintDefaults` and `flag.VisitAll`.
