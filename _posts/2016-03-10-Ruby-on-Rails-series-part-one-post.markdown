---
layout: post
title:  "Ruby on Rails Series - Part I"
date:   2016-03-10 11:30:21
categories: jekyll update
---

##Ruby on Rails - Part I

In the introduction for this series I talked about what Rails offers the developers, I'd like to extend from that a little bit and talk about the *MVC* pattern. 

###**Model - View - Controller** 

#### Model (ActiveRecord)
The Models are plain old Ruby classes. They inherit from ActiveRecord, *(which we'll see in more depth in future posts)*, which is the layer of the system responsible for representing business data and logic. ActiveRecord facilitates the creation and use of business objects whose data requires persistent storage to a database.

#### View (ActionView)

The View of the *MVC* pattern represents, well, the presentation layer for the application. It is responsible for rendering the data into one or more formats, such as XHTML, XML, or even Javascript(JSON). Rails gives us the `erb` tag, where we can embed Ruby in our views.

#### Controller (ActionController)

Finally the Controller connects the models and views. It receives events from the outsite world, *usually through the views*; interacts with the model and displays the appropiate objects to the views for it to be displayed.

We're going to talk about REST*ful* resources, in the near future. But the idea is that Rails gives us 7 actions *(index, show, new, create, edit, update, destroy)* known as **Create, Read(Retrieve), Update and Delete** (CRUD). When defining `resources :users` in our `routes.rb` We are able to use all 7 actions in our controller.

#### Why is this important?

The idea of separation of concern is really important for a maintainable code base.

The View does not have to be the one to get the data out of the database, since this is the Model's job, and the Controller decides how and what format of that data to pass on to the view (json, xml).
