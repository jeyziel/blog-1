+++
Categories = []
Description = ""
Tags = []
date = "2015-10-10T12:01:21-07:00"
title = "Defining Methods: Pointer Receivers and Value Receivers—Which Should You Choose?"
+++

Many people are scared of pointers when they first get started in Go, especially if they're coming to Go from dynamic languages that don't expose the concept. Even after they've gotten a good grasp on what pointers are and how they work, many new gophers feel a bit anxious about when they should be defining a method on a value rather than a pointer.

The Go team has given a good set of guidelines in their [FAQ][1], listing three basic considerations.

1. **Modification**: If you need to modify the receiver, then it must be a pointer.
2. **Efficiency**: If your value is large, use a pointer.
3. **Consistency**: Don't mix pointer / value receivers for a given type.

There are exceptions, but starting with these guidelines as a rule of thumb will get you pretty far.

Another excellent resource when it comes to making the decision about pointer vs value receivers is the [Code Review Comments wiki page][2] on the golang/go project on GitHub. It has a section about receiver types, which you should absolutely go read. It concludes with _when in doubt, use a pointer receiver_.

## Why Would You Ever Use a Non-Pointer Receiver?

When the receiver is a map or a slice you will almost always want to use a value receiver for these. Almost. Maps and slices act as references. [Go Slices: Usage and Internals][3] on the official Go blog is a fantastic deep dive into slices, and you should go read it if you haven't already, in order to understand why you occasionally will want to use a pointer receiver for a slice.

Aside from types that act as references, the most common case for defining a value receiver rather than a pointer receiver is when you are defining a small struct type that acts as a value. The term _value_ here might be a bit confusing, because this "value" is not the same "value" as in _value receiver vs. pointer receiver_.

This is about identity.

If you have two values of a given type, and all their fields have the same values in them, do they represent the same thing?

Sometimes the answer is yes. For example, 2:15pm is 2:15pm, provided that they're both in the same timezone. You might have a bunch of copies of this value, each copy might live in a different place in memory, but they're all equivalent.

This isn't always the case. For example you could have two people, both named Bob Smith, and both born on the same day. That doesn't mean that they're the same person, as anyone married to a Bob Smith can attest to. You can't just trade one Bob Smith for another.

Dates, times, points, vectors, web-safe colors are all typically small, and their identity is based on their value.

It's idiomatic in Go to define a value receiver rather than a pointer receiver for these types.

Some people ask: _Isn't it always more efficient to use a pointer receiver?_

Two answers:

1. Nope.
2. You profiled, right?

A pointer receiver introduces indirection. In some cases this indirection has a measurable cost.

&gt; The problem most people have, is that they try to make this decision based on what they think the performance tradeoff will be. – William Kennedy, [Using Pointers in Go][4]

It's far better to make choices based on what is simple and idiomatic, than on some pre-conceived idea of what will perform well.

To quote the [Rules of Optimization][5]:

1. The First Rule of Optimization: Don't.
2. The Second Rule of Optimization: Don't… yet.
3. Profile before optimizing.

[1]: https://golang.org/doc/faq#methods_on_values_or_pointers
[2]: https://github.com/golang/go/wiki/CodeReviewComments#receiver-type
[3]: http://blog.golang.org/go-slices-usage-and-internals
[4]: http://www.goinggo.net/2014/12/using-pointers-in-go.html
[5]: http://c2.com/cgi/wiki?RulesOfOptimization
