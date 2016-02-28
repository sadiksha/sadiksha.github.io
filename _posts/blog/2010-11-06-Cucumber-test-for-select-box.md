---
layout: post
title:  "Cucumber Test for Select Box"
date:   2010-08-29
categories: blog
---


The behavior driven development is really fun. First of all you determine the flow of the program and then only write real codes. This technique is really effective as it saves time while programming, since the flow is already determined, the chance of hindrance is very low.

A simple test for seeing something in select box is:

{% highlight text %}
Given I am on the index page
And the "Nepal" should be selected for "Country"
{% endhighlight %}

(where "Nepal" is your selected value and "Country" is the id/label of the select box)

The step definition for this step is

{% highlight text %}
Then /^"([^\"]*)" should be selected for "([^\"]*)"$/ do |selected_value, field|
  find_by_id(field).value.to_s.should == selected_value
end
{% endhighlight %}
