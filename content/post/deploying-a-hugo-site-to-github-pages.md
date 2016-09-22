+++
Categories = ["hugo"]
Description = ""
Tags = []
date = "2015-11-27T08:26:28-07:00"
title = "Deploying a Stand-Alone Hugo Site to GitHub Pages Mapped to a Custom Domain"
+++

If you already know how to use Hugo, and you've never used GitHub Pages before, and
you just want to figure out how to get everything wired up and deployed with the least
amount of fuss necessary, then a lot of the blog posts and tutorials you'll find are
going to be a bit frustrating.

Some tutorials will explain the GitHub Pages part in detail, but will make assumptions
about how you're generating your site, which won't necessarily match what Hugo does.

Other tutorials will assume that you know _nothing_, and will explain every step of
everything from scratch.

This post assumes that you know what all the pieces are (hugo, repositories, a bit of DNS),
and you just want to figure out the easiest way to stitch it all together.

First, a warning:

## Don't Make a `gh-pages` Branch

A lot of the documentation you'll find will talk about creating a branch
called `gh-pages` for the HTML. This is great if you're creating a portfolio
site with sub-sites for different projects on GitHub.

If you're looking to create a stand-alone site mapped to a custom domain,
then that is not the documentation you need.

The `gh-pages` branches are what GitHub refers to as _Project Pages_. What
you need when creating a stand-alone site are _User Pages_ or _Organization Pages_
(which are the same thing, it just depends on if your GitHub user is a user
or an organization).

Ok, moving on.

## A Tale of Two Repositories

The trick to deploying a stand-alone, hugo-generated site that will be hosted
on a custom domain is that everything within `public/` needs to be in its own
repository, and that repository _must_ be named `<username>.github.io`, where
`<username>` is your actual GitHub username.

This means that all the markdown and templates and configuration needs to go in
a separate repository. The repository with all the Hugo stuff can be named
whatever you like. For the sake of argument, let's assume this repository is called `blog`.

The initial setup depends on what your current situation is. Most likely, either:

1. nothing is committed to source control yet, or
1. you already have your hugo site committed and pushed up to the `blog` repository
   on GitHub.

### Setup When Nothing is Committed

Create two new, empty repositories on GitHub:

1. `blog`
1. `<username>.github.io` Make sure to check the *Initialize this repository with a
   README* box, since that will make the next step easier.

Kill your hugo server so that it stops regenerating the HTML.

Delete the `public/` directory with `rm -r public/`.

Initialize a git repository and add the remote:

    $ git init
    $ git remote add origin git@github.com:<username>/blog.git

### Setup When You've Already Committed and Pushed

If you've already got your Hugo site committed to source control and pushed up to
GitHub, then the process is similar, except that you need to make room for the submodule
that you're going to add right after the setup is complete.

Kill your hugo server so that it stops regenerating the HTML.

Create a new, empty repository named `<username>.github.io` on GitHub, making sure
to tick the *Initialize this repository with a README* box.

Delete the `public/` directory from git:

    $ git rm -r public

### Adding the Submodule

Clone the `<username>.github.io` repo into a submodule in `public`:

    $ git submodule add git@github.com:<username>/<username>.github.io.git public

Add everything and push it up to GitHub:

    $ git add .
    $ git push -u origin master

### Deploying

Regenerate the HTML and push the submodule up to GitHub:

    $ hugo -t -theme=YOURTHEME
    $ cd public
    $ git add .
    $ git commit -m "Generate site"
    $ git push origin master

You should be able to see the index page up on <username>.github.io a few moments later.

Add a handy deploy script like [Spencer Lyon's script][deploy] to simplify things a bit.

## Mapping a Custom Domain

Whether you're going to use a subdomain like `blog.yoursite.com` or an apex domain like
`yoursite.com`, you need to first add a file named CNAME to the submodule repository containing
the domain you're mapping to.

Note that this file should be named `CNAME`, even if the DNS record you're creating is an A record
or ALIAS record rather than a CNAME record.

If you're mapping a subdomain, create a CNAME record with your DNS provider. For an apex domain
you'll need either an ALIAS record on an A record. It depends on the provider.

For more information about DNS mappings, check out the [guide on GitHub][dns].

## Regenerating All the URLS

Once the DNS has propagated you're going to need to change the base host name in the Hugo config
file, regenerate the site with the correct urls, and redeploy.

[hugo]: http://gohugo.io/
[deploy]: https://github.com/spencerlyon2/hugo_gh_blog/blob/master/deploy.sh
[dns]: https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages/
