+++
Categories = []
Description = ""
Tags = ["cli"]
date = "2015-10-05T11:58:09-07:00"
title = "Silence Noisy Output from Command-Line Tools by Monitoring Progress with SIGINFO"
+++

Every few weeks the team changes what data needs to be exposed in the app. Every few weeks you need to re-run the cache-warmer so that Elasticsearch has the right data, and the website can serve snappy pages without making all the slow, complicated queries on the fly.

Every day, the data grows, and the task is taking longer and longer to run.

The longer it takes, the antsier the team gets, wondering if perhaps it's not working. Everyone recalls that one time they[1] accidentally forgot to increment the counter and spent hours and hours processing the same 1000 rows over and over again.

It's very, very tempting to print progress reports:

    indexed report 1
    indexed report 4
    indexed report 10
    deleted report 11
    deleted report 27
    indexed report 28
    ...
    indexed report 31202381

Or, if you don't want to spew 31 million rows of output into your terminal, you might only report once every 1000 records. Or you could have a progress bar.

Once I wrote a tool that reported `.` or `^` depending on what was done with the record. Screenfuls and screenfuls.

&gt; Dearest authors of command-line tools: please keep the output short and to-the-point. In fact, it's best to only print when things go wrong. – [Andrew Gerrand][1]

A better alternative would be to accept a signal, e.g. `SIGINFO`, and only report progress when that particular signal is received.

Some terminals translate ctrl-t into a `SIGINFO`, which means that you don't even have to figure out what process id to send the signal to.

If ctrl-t doesn't work on your system, then you can always get the process id with `ps`, and send the signal manually.

    kill -SIGINFO 68917
    Processed up to: 81

Or, you could use `os.Getpid()` to get the program to report its own process id on startup, though that kind of defeats the purpose.

The standard library has fine-grained support for giving your program access to the signals that you're interested. Check out `go doc signal.Notify` and `go doc syscall.Siginfo` for more information.

Here's a sample program that puts it all together.

    package main

    import (
            "fmt"
            "os"
            "os/signal"
            "syscall"
            "time"
    )

    func main() {
            var i uint64 = 0

            sigs := make(chan os.Signal, 1)
            signal.Notify(sigs, syscall.SIGINFO)
            go func() {
                    for {
                            &lt;-sigs
                            fmt.Printf("Processed up to: %dn", atomic.LoadUint64(&amp;i))
                    }
            }()

            // simulate doing lots of hard work for 60 seconds
            go func() {
                    for {
                            atomic.AddUint64(&amp;i, 1)
                            time.Sleep(500 * time.Millisecond)
                    }
            }()
            time.Sleep(60 * time.Second)
    }

1) _Yeah, sorry about that._

[1]: https://twitter.com/enneff/status/537572707731128320
