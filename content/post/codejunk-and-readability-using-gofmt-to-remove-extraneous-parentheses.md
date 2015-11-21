+++
Categories = []
Description = ""
Tags = ["readability", "tools"]
date = "2015-10-26T12:13:34-07:00"
title = "Codejunk and Readability: Using `gofmt` to Remove Extraneous Parentheses"
+++

You start out with just the right number of parentheses: the bare minimum. Well, as few as you can get away with, without having to worry about remembering operator precedence. (_Does `||` bind more tightly than `&&[`1]?_)

Then edge cases happen. You wrestle with logic, try out variations, and parentheses accrete.

Bugfix 1, simplicity 0.

This leaves you trying to spot the actual logical expressions amidst all the spurious parentheses.

Noise, in other words. Codejunk[2]. Superfluous visual elements that get in the way of comprehension.

You might think that `gofmt` will take care of it automatically, but if `gofmt` removed all the unnecessary parentheses, then readability would take a hit. We'd all be back to remembering operator precedence[3].

Even though `gofmt` won't do this automatically, the command has a flag that will let you ask it to remove extraneous parentheses: `-r`, for _rewrite_.

Take this bit of code, observed in the wild:

    func IsLeapYear(y int) bool {
    	return ((y%4) == 0 && (!((y%100) == 0 && (y%400) != 0)))
    }

Here's the command that will ask `gofmt` to show a diff with the extra parentheses removed:

    gofmt -d -r '(a) -&gt; a' .

Running this against the above code provides the following diff:

     func IsLeapYear(y int) bool {
    -	return ((y%4) == 0 && (!((y%100) == 0 && (y%400) != 0)))
    +	return y%4 == 0 && !(y%100 == 0 && y%400 != 0)
     }

This is significantly easier to read. In fact, now that the logical expression is readable, it's possible to see that the entire thing can be further simplified.

    y%4 == 0 && y%100 != 0 || y%400 == 0

Check out <a href="http://`gofmt`" target="_blank">the gofmt documentation for details about the rewrite flag.

* * *

1) No.
2) A play on Edward Tufte's _Chartjunk_. I first heard the term codejunk from Carl Manaster in 2009.
3) To be fair, that's actually [not difficult in Go][1].

[1]: https://golang.org/ref/spec#Operator_precedence

