+++
Categories = []
Description = ""
Tags = ["idioms"]
date = "2015-10-16T12:09:52-07:00"
title = "Itty Bitty Go Idiom: If Without Else"
+++

Writing code in a new language can sometimes make you feel like an introvert at a party where you don't know anyone, in a country where you're not sure about the customs.

For the most part you're not going to insult anyone too badly, and you'll probably have a reasonably good time, even if occasionally people will think that you're quaint or a bit daft or have kind of bad manners.

Usually the things you're doing wrong are going to be things that never, ever occurred to you to even think about. For example, if you're an American in France, try not to smile at strangers all the time.

With programming you're probably not worried about insulting someone with outlandish habits, but you might be feeling a bit awkward and uncertain. The code works as intended, but you're not sure if you're doing things the right way. The Go way.

You will think to ask about all sorts of things: `CamelCase` or `snake_case` or `ALL_CAPS`? Tabs or spaces?

It might never occur to you that the `if`/`else` that you've used in every single language that you've written code in (except that one project in SmallTalk) might be a tip-off that you're new around these parts.

While it's not considered bad practice to use `else`, it's actually fairly uncommon to see an `else` or `else if`.

> Try to keep the normal code path at a minimal indentation, and indent the error handling, dealing with it first.
> - [CodeReviewComments][1]

In other words, you're going to see a lot of this:

    if someErrorCondition {
        // handle the sad path
        return
    }
    // do the happy path thing

And not a whole lot of this:

    if allGood {
      // yepp
    } else {
      // nope
    }

[1]: https://github.com/golang/go/wiki/CodeReviewComments#indent-error-flow
