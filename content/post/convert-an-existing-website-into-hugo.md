+++
Categories = ["hugo"]
Tags = []
Description = ""
date = "2016-09-22T06:11:01-07:00"
title = "Convert an Existing Site into Hugo"
+++

There are times when all you need is a bit of hand crafted HTML. A home page. An about page. Maybe one to list your favorite brownie recipe.

And then there are times when you _thought_ all you needed was a brochure site, and a few months later you realize that every few days you're adding another page, which involves annoying things like copy/pasting the same header and footer everywhere, and fiddling with navigation.

When it was just going to be three pages, writing out all the HTML for the prose was fine. Now, not so much. Now you're aching for markdown, partials, and navigation that takes care of itself.

You could write a full-on dynamic site, but it would be overkill. You don't need dynamic content, you need HTML that doesn't suck.

[Hugo][hugo], the delightfully easy to use static site generator, is a great choice, but the documentation assumes that you'll be using an existing template. If you've already got a perfectly good site design, then a third-party template is more trouble than it's worth.

Instead, you want to reconfigure your existing website so that hugo _generates that_. Adding new pages will be a simple matter of adding a new markdown file. Everything else should take care of itself.

Here's the plan:

1. Wire hugo up to generate your existing website without any changes.
1. Extract shared markup into partials.
1. Create a template that can generate a basic page.
1. Convert each page into markdown.

## Wire Up Hugo

Generate a brand new hugo site:

```
$ hugo new site yoursite
$ cd yoursite
```

Delete the generated static directory:

```
$ rmdir static
```

Copy your entire old site to be the new static directory:

```
$ cp -r ~/path/to/oldsite static
```

Finally, move your index page into the layouts directory:

```
$ mv static/index.html layouts/
```

You can now run `hugo server` to view your site at on [localhost][localhost], or run `hugo` to generate the full site to the `public/` directory.

## Extract Partials

The next step is to create partials for the header and footer.

They're going to go in `layouts/partials`:

```
layouts/partials/
├── footer.html
└── header.html
```

Copy the header stuff from `layouts/index.html` into `header.html` and the footer stuff into `footer.html`.

Then replace the header stuff and footer stuff in `layouts/index.html` with calls to `partial`:

```
{{ partial "header.html" }}

<!-- the main part of the page -->

{{ partial "footer.html" }}
```

If you visit [localhost][localhost] everything should still look right. You should also still be able to generate the site to `public/` with the `hugo` command.

## Create a Page Template

Create a new file at `layouts/_default/single.html` and give it the header, footer, and a placeholder for the content:

```
{{ partial "header.html" }}

{{ .Content }}

{{ partial "footer.html" }}
```

You might have more boilerplate you want to stick around the `.Content` directive, but this is the basic idea.

## Convert Existing Pages

Now, one by one, you can turn a static HTML page into a bit of markdown that uses your new template.

For example if you have an about page at `static/about/index.html`, you can now create `content/about.md` with just the meat of the page. You can actually copy the HTML straight to `about.md` as a first step. It will generate it just fine. Then turn the markup into markdown.

## Add New Content

At this point you'll be able to add new markdown pages to `content/` and they'll be generated correctly.

[hugo]: https://gohugo.io/
[localhost]: http://localhost:1313/
