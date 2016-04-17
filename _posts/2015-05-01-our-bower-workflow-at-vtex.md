---
layout: post
title: "Our (old) Bower workflow at VTEX"
permalink: /blog/7/our-bower-workflow-at-vtex/
redirect_from:
  - /blog/7/
---

*This was an internal email I wrote to the frontend team at [VTEX](http://lab.vtex.com/)
in an effort to consolidate our Bower practices, originally published on 29 Jul 2013.*

[Bower](https://github.com/bower/bower) is a "package manager for the web". It functions by downloading components that your project depends on, like jQuery or Underscore. These dependencies are defined in a `bower.json` file. Each dependency is either: 

 - a name that maps to a package registered with Bower, e.g, `jquery`
 - a remote Git endpoint, e.g., git://github.com/someone/some-package.git, which can be public or private
 - a local Git endpoint, i.e., a folder that's a Git repository
 - a shorthand endpoint, e.g., someone/some-package (defaults to GitHub)
 - a URL to a file, including zip and tar files, which's contents will be extracted

Example configuration file:

{% highlight json %}
{
  "name": "portal-plugins",
  "version": "1.0.0",
  "dependencies": {
    "jquery": "1.8.3",
    "front-utils": "https://github.com/vtex/front.utils.git"
  }
}
{% endhighlight %}
    
After you run bower with `bower install`, bower will download the various dependencies (recursively, too) and put them in the `bower_components` directory.

Let's see how we can define a project of our own as a Bower package to be consumed by others (e.g., front.utils).


Package definition
-----------------

A Bower package must also have a `bower.json`. It can also have dependencies. Preferably, it should have a `main` property that identifies the main file(s) in the package.


{% highlight json %}
{
  "name": "front.utils",
  "version": "0.1.0",
  "main": ["dist/vtex-utils.js", "dist/vtex-utils.min.js"],
  "dependencies": {
    "underscore": "latest"
  }
}
{% endhighlight %}
    

The problem
-----------

Bower does its job by downloading the *entire repository* of each dependency -- this may include tests, documentation, uncompiled files (e.g, coffeescript or less), and meta files (e.g., .gitignore).

Evidently, all this bloat is unnecessary: we just need that little `jquery.min.js` file in our libs folder.

You may think that the `main` property solves this -- but Bower simply ignores it. We need another tool that makes good use of it - enter bower-install.


The solution: [bower-installer](https://github.com/blittle/bower-installer)
----

This is a tool that acts as an extension to Bower, and is just the solution we need to our problem.

Its main command, `bower-install`, runs the `bower install` command, then grabs each package's main files and put them in a centralized folder somewhere else, like `src/lib/bower_modules`.

This is how you would configure your `bower.json` in order to be able to use this tool:


{% highlight json %}
{
  "name": "portal-plugins",
  "version": "1.0.0",
  "dependencies": {
    "front-utils": "https://github.com/vtex/front.utils.git"
  },
  "install": {
    "path": "src/lib/bower_modules"
  }
}
{% endhighlight %}
    
Some observations about the tool:

 - It does not delete your `bower_components` folder, so you may want to do that yourself
 - It a `bower_components` folder already exists, it doesn't do `bower install` again (so you REALLY want to remove that folder yourself)
 - It cleans up the install folder, so define it to somewhere that is used exclusively by this guy
    
That's it, you just need to add an aditional `install` property that has a `path` property. For more complex configuration, refer to the documentation. 

Wrapping up
---

With all being well understood, let's define some conventions to use in our team.

 - The bower folder should be the default (`bower_components`)
 - The install path should be `src/lib/bower_modules`
 - The distribution path should be `dist`

Let's all use this shell script to automate our bower workflow:


{% highlight bash %}
#!/usr/bin/env bash
sudo rm -rf bower_components/
bower-installer
sudo rm -rf bower_components/
{% endhighlight %}
