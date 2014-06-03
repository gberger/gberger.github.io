---
layout: post
title: "How to have multiple compiled assets in Rails"
permalink: /blog/3/how-to-have-multiple-compiled-assets-in-rails
---

The default Rails configuration is to have one big application*.js file, containing all your scripts, including those that are specific to admin pages.

Users that are not admins will still be loading the admin scripts. There are a few problems with this:

 * It increases their load time. In my case, I have over 100kB in libraries that are only used in admin pages.

 * It can leak sensitive information.

Because of these reasons, I decided to separate my public and admin scripts (and stylesheets) into two separate files. **This is how I did it.**

First, structure you application like this. Please note that all admin-specific javascript files must go in the javascripts/admin folder, and everything else should go inside javascripts/public. The same is true for your stylesheets.

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

In **admin.js**:

    //= require_tree ./admin

In **public.js**:

    //= require_tree ./public

In **admin.css**:

    /*
     *= require_directory ./admin
     */

In **public.css**:

    /*
     *= require_directory ./public
     */

In **config/environments/production.rb**, add this line inside the **configure** block:

      config.assets.precompile += %w( public.js admin.js public.css admin.css )

In **app/views/layouts/application.html.erb** (or whatever your main layout file is):

    <!-- Change this: -->

      <%= stylesheet_link_tag "application", media: "all" %>
      <%= javascript_include_tag "application" %>


    <!-- To this: -->

      <%= stylesheet_link_tag "public", media: "all" %>
      <%= javascript_include_tag "public" %>

The final step is to include similar CSS/JS tags in pages that need the admin files. There are a few ways to do this.

You could add them manually in every view that is admin-facing. This is easy if every admin view shares a same layout.

    <%= stylesheet_link_tag "admin", media: "all" %>
    <%= javascript_include_tag "admin" %>

Another way is to load these files in every page, but only if the current user is an admin. The problem with this approach is that admin users will have bigger loading times.

    <% if current_user && current_user.is_admin? %>
      <%= stylesheet_link_tag "admin", media: "all" %>
      <%= javascript_include_tag "admin" %>
    <% end %>

There are many other ways to execute this final step. Maybe you have some variable that says whether you are in an admin page or not, if your application is structured to allow that. Anyway, find out what is the best solution for you and use it.
