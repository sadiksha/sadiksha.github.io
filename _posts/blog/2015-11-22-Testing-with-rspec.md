---
layout: post
title:  "Testing with Rspec"
date:   2015-11-22
categories: blog
---

Test driven development (TDD) is a concept of software development in which a new feature is developed by writing tests first, making it fail, and writing the code to make the test pass. This process continues iteratively until all the tests pass.

Starting to code with TDD enables developers to think about the test cases first rather than diving immediately in writing the code for the feature. In this case, the requirements and the scenarios are thought about in advance. In this blog, I will go through installing rspec and adding doing TDD. Rspec is used for writing automated tests.

First of all, I will start with rspec-rails.

1) Add following to the Gemfile:

{% highlight text %}
  group :test do
    gem 'rspec-rails'
  end
{% endhighlight %}

2) *bundle install*

3) *rails generate rspec:install*

This will create certain files like *spec_helper.rb* in *spec/*. This file contains the configuration for the test. For instance, the database cleaner for cleaning test database is added in spec_helper. For more details, please refer here. Let us assume that, we have a model Post and it has attributes title and description. Add a file post_spec.rb: spec/models/post_spec.rb

{% highlight text %}
require "spec_helper"

describe "Post", :type => model do
  it "has attributes name and author" do
  post = Post.new(title: "New Post", description: "This is a new post")
  post.save

  expect(post.attributes).to include("title")
  expect(post.attributes).to include("description")
  expect(Post.count).to be(1)
  expect(Post.first.title).to be("New Post")
  expect(Post.first.description).to be("This is a new post")
  end
end
{% endhighlight %}

When you run the rspec test with command

{% highlight text %}
  bundle exec rspec
{% endhighlight %}

There will be an error:

{% highlight text %}
  Uninitialized constant Post
{% endhighlight %}

Then you create a model with

{% highlight text %}
  rails g model Post
{% endhighlight %}

Then when you run the test again it will give an error saying:

{% highlight text %}
  Undefined method title for Class
{% endhighlight %}

So add a new migration to add attributes title and description to the model Post with

{% highlight text %}
    rails generate migration CreatePosts title:string description:text
{% endhighlight %}

This will create a migration file in *db/migrations/xxxxxxxxxxxxxx_create_posts.rb* (xxxxxxxxxxxxxx is the timestamp the migration is created). When you open this file, it will look like:

{% highlight text %}
class CreatePosts < ActiveRecord::Migration
  def change
    create_table :posts do |t|
      t.string :title
      t.text :description
      t.timestamps
    end
  end
end
{% endhighlight %}

Then run the migration with:

{% highlight text %}
  bundle exec rake db:create
{% endhighlight %}

(for the first time) and

{% highlight text %}
  bundle exec rake db:migrate
{% endhighlight %}

Then, getting back to the test again. Now when you run rspec again, the tests again fail and it says as you will have to define the attributes in model as well.

Add the following lines in your *app/models/post.rb*

{% highlight text %}
  attr_accessible :title, :description
{% endhighlight %}

Now, when you run the test again with

{% highlight text %}
  bundle  exec rspec
{% endhighlight %}

your tests successfully pass. Congratulations, you just did TDD with rspec.
