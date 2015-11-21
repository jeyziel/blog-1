+++
Categories = ["documentation"]
Description = ""
Tags = ["documentation", "godoc"]
date = "2015-09-27T10:43:50-07:00"
title = "How to Run the Go Tour Locally"
+++

You decided to give Go a try. You installed the language on your Mac, you configured a `$GOPATH`, and the internet told you that you should take the Go Tour next.

Pretty much everywhere they mention the tour, they also tell you that you can install it locally. So you ran the command:

      go get golang.org/x/tour/gotour

It clearly worked, because `ls $GOPATH/src/golang.org/x/tour/gotour` has the downloaded code, but now you're left with what feels like a silly question:

> How do you actually get it to run?

The short answer is that `go get` installed a binary into `$GOPATH/bin`, so you can run it with

      $GOPATH/bin/gotour

That should pop open a browser window to  and you should be all set.

The longer answer is that how you run it depends on your `PATH`.

## PATH—Where Programs Live

The `PATH` is a list of places on your filesystem that your computer will look, if you ask it to run a program.

You can take a look at your `PATH` environment variable by running the command

      echo $PATH

It will look something like this:

      /Users/you/bin:/usr/local/go/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin

Except it will probably be much, much longer and have some pretty gnarly looking filepaths. Each filepath is separated by a :.

So with the path above, if you type gotour in a terminal window, then it will look each of the locations in order, stopping (and running the command) if it finds the program:

* `/Users/you/bin`
* `/usr/local/go/bin`
* `/usr/local/bin`
* `/usr/bin`
* `/bin`
* `/usr/sbin`
* `/sbin`

Let's say that your `$GOPATH` resolves to `/Users/you/code/go`. This means that gotour will be installed in `/Users/you/code/go/bin`, which is not in the list, and the computer comes up empty.

You'll get some sort of message saying that the command is not found. That's fine. You can always tell the computer exactly where to find the program by specifying the full path.

      /Users/you/code/go/bin/gotour

You can also use a relative path.

      ../../../bin/gotour

Or, if you are in the same directory as the program, then you can prefix the name of the file with `./`.

      ./gotour

Since you are likely to run a lot of Go binaries, you might want to add `$GOPATH/bin` to your `PATH`. This is really handy, because it lets you run a binary from anywhere on your system without specifying where to find it, just by typing the name.

      gotour
