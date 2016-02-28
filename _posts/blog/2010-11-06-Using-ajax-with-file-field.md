---
layout: post
title:  "Ajax with file field"
date:   2010-08-28
categories: blog
---

Sometime ago I had real trouble using file field and ajax call together. I wanted to browse a file and display the contents in the same page. Then after some research I came across gem called [remotipart](https://github.com/JangoSteve/remotipart). My problem was solved with this gem.

To use this gem follow these simple steps

1. Include *gem remotipart* in your Gemfile
2. Run *bundle install* from your terminal
3. Generate remotipart for rails by running following from your terminal

{% highlight text %}
 rails generate remotipart
{% endhighlight %}

4. Include *jquery-1.4.2-min.js*, *rails.js*, *jquery.form.js* and *jquery.remotipart.js* as your javascript files
5. Suppose you are using this in your *new.html.erb file*, the form should look like the form below

{% highlight text %}
<%= form_for @product, :html => { :multipart => true }, :remote => true do |f| %>
<%= f.label :file_upload %>
<%= f.file_field :file_upload %>
<%= f.submit %>
<% end %>
{% endhighlight %}

6. In your create controller include: format.js {} instead of format.html{}
7. In *create.js.erb* file :

{% highlight text %}
<%= remotipart_response do %>
//Write your javascript code here.
<% end %>
{% endhighlight %}

I hope this post helped you.
