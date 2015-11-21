+++
Categories = ["documentation"]
Description = ""
Tags = ["tools"]
date = "2015-09-29T10:57:09-07:00"
title = "How to View the Documentation for a Single Standard Library Function on the Command-Line"
+++

You know exactly which function you need to use, and you know which package it's in, and you seem to recall that there's a gotcha with it, but for the life of you, you just cannot remember the details.

Your first instinct might be to hit up a search engine: _golang filepath walk_.

That's fine, and it will get you what you need quickly, along with about 30,000 other results.

If all you want is the language documentation, though, you can get exactly what you need right on the command-line.

    go doc filepath.Walk

The documentation will show up right in the shell:

    package filepath // import "path/filepath"

    func Walk(root string, walkFn WalkFunc) error

        Walk walks the file tree rooted at root, calling walkFn for each file or
        directory in the tree, including root. All errors that arise visiting files
        and directories are filtered by walkFn. The files are walked in lexical
        order, which makes the output deterministic but means that for very large
        directories Walk can be inefficient. Walk does not follow symbolic links.

There's that gotcha: It won't follow symlinks.

As an added bonus, this also works with your own libraries

    go doc bubbles.Embellish

    package bubbles // import "github.com/kytrinyx/fish/bubbles"

    func Embellish(s string) string

        Embellish decorates the string with ascii bubbles.

It's pretty nifty!

**Note: the `doc` subcommand was introduced in Go 1.5.** If you are using an older version of Go you can use the `godoc` command, which takes the full import path to the package:

    godoc path/filepath Walk
