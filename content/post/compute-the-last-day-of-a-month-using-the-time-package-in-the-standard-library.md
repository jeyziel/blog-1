+++
Categories = []
Description = ""
Tags = ["stdlib"]
date = "2015-10-09T11:59:48-07:00"
title = "Compute the Last Day of a Month using the `time` Package in the Standard Library"
+++

If you're having trouble finding the convenience function in Go to compute the last day of the month, it's because it's not there. Libraries have been written to add specialized time and date functions, but you really don't have to reach for an external dependency.

Here are three ways to compute the last day of the any month using the existing functionality in the `time` package in Go's standard library.

## The 32nd Day

Ian Lance Taylor suggested a trick where you get the 32nd day of the month you're interested in, which becomes the _n_th day of the next month. Then you can subtract 32-n to get the value you're looking for.

    32 - time.Date(2016, time.February, 32, 0, 0, 0, 0, time.UTC).Day()

## The Zeroth Day

An even simpler trick, suggested by a fellow named Johnathan is to use the "zeroth" day of the following month, which by definition is the last day in the month you're interested in.

    time.Date(2012, time.February+1, 0, 0, 0, 0, 0, time.UTC).Day()

## A Month—Less a Day

If you have a value of type `time.Time` representing the first day of the month, then you can use `AddDate()` to get the value that you want:

    ts := time.Date(2008, time.February, 1, 0, 0, 0, 0, time.UTC)
    ts.AddDate(0, 1, -1).Day()

Or, you could use the timestamp with either of the previous methods by accessing the month and year:

    ts.Year() // 2008
    ts.Month() // 2

## Don't Reach for that Dependency

Before reaching for a 3rd party library or package, take a good look at what the standard library can offer.

Just because Go's [`time` package][1] doesn't have a convenience function, doesn't mean that it's not convenient to use it.

[1]: https://golang.org/pkg/time/
