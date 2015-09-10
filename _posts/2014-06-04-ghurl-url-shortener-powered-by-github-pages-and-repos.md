---
layout: post
title: "ghurl URL Shortener"
description: "A URL Shortener with no back-end server"
permalink: /blog/5/ghurl-url-shortener
redirect_from:
  - /blog/5/
---

I wanted to explore ways I could (ab)use the GitHub pages and
repositores ecosystem by creating working systems that would otherwise
need a back-end server.

I was inspired by [GitHub Resumé](https://github.com/resume/resume.github.com),
which uses an opt-in system based on starring the repository. Other sources of
inspiration were repositories that use pull requests to maintain information about,
e.g., a tech meetup group in a certain city.

I created **[ghurl](http://ghurl.github.io/)**, a URL shortener. My idea was
to use GitHub issues, combined with the GitHub API, to store and retrieve the long URLs.

*Here's a [URL pointing to this post](http://ghurl.github.io/?6), and here's
the [GitHub issue](https://github.com/ghurl/db/issues/6) it refers to.*

The user's flow goes like this:

1. Create an issue on the [ghurl/db](https://github.com/ghurl/db/issues) repository.
   The **title** of the issue should be the URL you want to redirect to.

2. Note **n**, the **issue's number**.

3. Construct your shortened URL using **n**:
   `https://ghurl.github.io/?n`

Then, when the user accesses the "shortened URL" (it's not really that short,
unless I buy a custom domain), I use the issue's number (from the query string)
to send an AJAX call to the GitHub API. When it's done, I use the issue's title as the
URL to redirect the user to.

The short URLs contain the issue number in the query string, so
I could later retrieve them from the repo. And by using a query string,
I effectively have a catch-all URL into a single HTML page: perfect
for using GitHub pages for hosting the static HTML+JS+CSS files.

Of course, nobody should really use this service in the real world, for several reasons:

* It needs an extra network trip to fetch the issue
* It is kinda hard to use and requires that the user is familiar with GitHub and GitHub issues
* It doesn't provide analytics or tracking. It allows the user to change the target URL, though (by editing
the issue).

However, I still plan to improve its usability a little bit. Here's what's on my mind:

* Text input with button that leads to `https://github.com/ghurl/db/issues/new?title=http://myurl.com`
* Text input for the issue number, live changing the short URL
* Easy to copy short URL
* Automatically capture issue number (call API for all issues, find by title == given URL)

It was fun to bend the environment to support a minimalistic system, and I look forward
to tackling a new challenge.
