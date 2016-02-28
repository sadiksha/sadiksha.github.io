---
layout: post
title:  "Deploy mailman with daemon and capistrano"
date:   2016-02-21
categories: blog
---

Recently, I wanted to store the contents of the email I receive on my email to database of my rails application. I was able to achieve my goal with mailman gem. I learned about it from railscasts. In this post, I will write about how I implemented it.

I added following to my *Gemfile*

{% highlight text %}
  gem mailman
{% endhighlight %}


After that, I ran bundle install from my terminal.

Then, I added the following code to my *script/mailman_server.rb*:

{% highlight ruby %}
  #!/usr/bin/env ruby require "rubygems"
  require "mailman"
  require File.expand_path(File.join(File.dirname(__FILE__), '..', 'config', 'environment'))

  Mailman.config.rails_root = '.'
  Mailman.config.logger = Logger.new("log/mailman.log")
  Mailman.config.pop3 = {
    server: 'mail.your-server.de', port: 110,
    username: 'your_username',
    password: 'your_password'
  }

  Mailman::Application.run do
    default do
      begin
        MyModel.receive_mail(message)
      rescue Exception => e
        Mailman.logger.error "Exception occurred while receiving message:\n#{message}"
        Mailman.logger.error [e, *e.backtrace].join("\n")
      end
    end
  end
{% endhighlight %}

I wanted to save the body of the mail to body attribute of my MyModel database. So, the model MyModel consists of receive_mail now looks like following:

{% highlight ruby %}
  def self.receive_mail(message)
    message_text = message.multipart? ? (message.text_part ? message.text_part.body.decoded : nil) : message.body.decoded
    my_model_params = { body: message_text}
    MyModel.create!(my_model_params)
  end
{% endhighlight %}

With the program written above, I stored the contents of the email that I received at username as mentioned in *mailman_server.rb* in my *my_model* table of the database.

Now, I wanted to run the *mailman_script.rb* script with daemons gem, so that I could run the script with just start, stop and restart commands.

In my *Gemfile*, I added the following line gem daemons and run bundle install.

I added a new file in the script folder called *mailman_daemon.rb*, that contains following code:

{% highlight ruby %}
  #!/usr/bin/env ruby

  require "rubygems"
  require "bundler/setup"
  require "daemons"

  Daemons.run('script/mailman_server.rb')
{% endhighlight %}

Now the hardest part that I had was setting up the capistrano with daemon. The main task is to start, stop and restart daemon with capistrano. So, that we don't have to start our daemon server with each deployment. I added the following to *config/deploy.rb*:

{% highlight ruby %}
  # Mailman configuration
  namespace :mailman do
    desc "Mailman::Start"
    task :start, :roles => [:app] do
      run "cd #{current_path};RAILS_ENV=#{rails_env} script/mailman_daemon.rb start"
    end

    desc "Mailman::Stop" task :stop, :roles => [:app] do
      run "cd #{current_path};RAILS_ENV=#{rails_env} script/mailman_daemon.rb stop"
    end

    desc "Mailman::Restart" task :restart, :roles => [:app] do
      mailman.stop mailman.start
    end
  end
  before "deploy:cleanup", "mailman:restart"
{% endhighlight %}

Now, the rails application is ready to be deployed with capistrano, mailman and daemon. Have fun!
