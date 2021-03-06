---
layout: post
title: "How to have multiple compiled assets in Rails"
permalink: /blog/3/how-to-have-multiple-compiled-assets-in-rails/
redirect_from:
  - /blog/3/
---

The default Rails configuration is to have one big application*.js file,
containing all your scripts, including those that are specific to admin pages

Users that are not admins will still be loading the admin scripts.
There are a few problems with this:

 * It increases their load time. In my case, I have over 100kB in libraries
that are only used in admin pages.

 * It can leak sensitive information.

Because of these reasons, I decided to separate my public and admin scripts
(and stylesheets) into two separate files. **This is how I did it.**

First, structure you application like this. Please note that all admin-specific
javascript files must go in the javascripts/admin folder, and everything else
should go inside javascripts/public. The same is true for your stylesheets.

{% highlight bash %}
- app
  - assets
    - javascripts
      - admin
        (all admin-facing js files go here)
      - public
        (all public-facing js files go here)
      admin.js
      public.js
    - stylesheets
      - admin
        (all admin-facing css files go here)
      - public
        (all public-facing css files go here)
      admin.css
      public.css
{% endhighlight %}

In **admin.js**:

{% highlight javascript %}
//= require_tree ./admin
{% endhighlight %}

In **public.js**:

{% highlight javascript %}
//= require_tree ./public
{% endhighlight %}

In **admin.css**:

{% highlight css %}
/*
 *= require_directory ./admin
 */
{% endhighlight %}

In **public.css**:

{% highlight css %}
/*
 *= require_directory ./public
 */
{% endhighlight %}

In **config/environments/production.rb**, add this line inside the **configure** block:

{% highlight ruby %}
      config.assets.precompile += %w( public.js admin.js public.css admin.css )
{% endhighlight %}

In **app/views/layouts/application.html.erb** (or whatever your main layout file is):

{% highlight erb %}
<!-- Change this: -->
<%= stylesheet_link_tag "application", media: "all" %>
<%= javascript_include_tag "application" %>

<!-- To this: -->
<%= stylesheet_link_tag "public", media: "all" %>
<%= javascript_include_tag "public" %>
{% endhighlight %}

The final step is to include similar CSS/JS tags in pages that need the
admin files. There are a few ways to do this.

You could add them manually in every view that is admin-facing. This is easy
if every admin view shares a same layout.


{% highlight erb %}
<%= stylesheet_link_tag "admin", media: "all" %>
<%= javascript_include_tag "admin" %>
{% endhighlight %}

Another way is to load these files in every page, but only if the current user
is an admin. The problem with this approach is that admin users will have bigger loading times.


{% highlight erb %}
<% if current_user && current_user.is_admin? %>
  <%= stylesheet_link_tag "admin", media: "all" %>
  <%= javascript_include_tag "admin" %>
<% end %>
{% endhighlight %}

There are many other ways to execute this final step. Maybe you have some
variable that says whether you are in an admin page or not, if your application
is structured to allow that. Anyway, find out what is the best solution for you and use it.
