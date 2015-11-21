+++
Categories = ["exercises"]
Description = ""
Tags = ["concurrency"]
date = "2015-10-13T12:04:23-07:00"
title = "Tiaras & Toolbelts: A Trivial Concurrency Exercise for the Confused Newbie Gopher"
+++

Sometimes you wonder if the program is mocking you.

&gt; all goroutines are asleep

Asleep. That sounds wonderful. Instead, here you are wondering whether deadlock would make a good bandname, and if you have any ketchup and mayonnaise left so you can make goopy red ramen noodles. Oh, yeah. And wondering why your app is hanging.

Perhaps all you need is a good night's sleep and a decent meal, and you'll find the unclosed channel or the `WaitGroup` that is missing a call to `Done()`, or whatever it is this time that has you perplexed and frustrated.

Or, maybe, you just need a break from name servers and other computer-science-y things so you can think about concurrency in the context of toys.

## Santa's Workers

Kraneisha is 4 years old. This year she has asked Santa for a tiara and a toolbelt so that she can be a princess mechanic.

She is one of about a million children who have asked for stuff. Things. Toys. Electronics. Blinky lights and obnoxious noisemakers.

The Elf typing pool has been normalizing requests into a text file:

    Kraneisha, 4, Tiara and toolbelt
    Eddie, 8, Chemistry set
    Ty, 5, Gaming console
    ...

**Write a program to simulate Santa's workshop**, where a single stream of requests gets portioned out to 1,000 elves to produce the gifts. Each completed gift is then shunted to the Wrapping Station, where they are packaged up and added to the sleigh.

Once all one million gifts wrapped and stowed, the sleigh can take off.

Don't worry about the mechanics of fitting one million gift-wrapped toys onto a sleigh. I'm pretty sure there's magic involved.
