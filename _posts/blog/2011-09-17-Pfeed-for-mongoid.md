---
layout: post
title:  "Pfeed for Mongodb"
date:   2011-09-17
categories: blog
---

I used [pfeed](https://github.com/parolkar/pfeed) for getting database changes in the view. The gem originally supports only ActiveRecord.

However, I needed the gem for mongodb. So, I changed the gem to make it compatible for mongodb.
The modified version of pfeed for mongoid can be found [here](https://github.com/sadiksha/pfeed). This is suitable for *mongoid version 2.0.2* and *rails 3.0.0*.
