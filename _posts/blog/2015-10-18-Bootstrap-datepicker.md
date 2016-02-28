---
layout: post
title:  "Bootstrap Datepicker"
date:   2015-10-18
categories: blog
---

I recently used [bootstrap datepicker](http://eonasdan.github.io/bootstrap-datetimepicker/) for selecting dates. As a gem, I used [bootstrap3 datepicker](https://github.com/TrevorS/bootstrap3-datetimepicker-rails) rails for my rails application. I needed the datepicker and time picker separately. So, I had two input fields, one just as calendar and another for time. This blog is about the datepicker. The timepicker will follow shortly. To use this gem, I added following to my *Gemfile*:

{% highlight text %}
    gem 'momentjs-rails', '>= 2.9.0'
    gem 'bootstrap3-datetimepicker-rails', '~> 4.14.30'
{% endhighlight %}

In the view, I include the datepicker as:

{% highlight html %}
    <div class="form-group col-md-6">
       <%= label_tag "date", "Date"%>
       <%= text_field_tag "date", nil, class:"form-control", placeholder: "From", required: true %>
       <span class="input-group-addon"></span >
    </div >
{% endhighlight %}

The above code shows only how the calendar will be displayed. To utilize the functionality of the calendar, we have to call datetimepicker() function of javascript. To use datepickers only as calendar use the following script:

{% highlight javascript %}
$(document).ready(function() {
  $(".date").datetimepicker({
    viewMode: 'days',
    format: 'DD/MM/YYYY'
  });
});
{% endhighlight %}

It does the following:

1. Give the date with format day/month/year. If the format is not selected, the calendar gives both calendar and time selection options.

2. Does not show time option so that you can only use the calendar.

Other lot of options are possible to configure the calendar. Please refer to the documentation if you intend to use it. I have added a small datetimepicker project [here](https://github.com/sadiksha/datepicker) so that it gives you an idea about how to use it in your rails application.
