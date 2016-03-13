---
layout: post
title:  "Ruby on Rails Series - Part II"
date:   2016-03-12 11:30:21
categories: jekyll update
---

### Ruby on Rails - Part II

#### Rails *New* Command & Options

In this part of the Rails series, I want to name a few commands that Rails gives us, as well commands that generates some code for us. 

1. First we'll start with `rails new <app_name>`.
  *  [`new`](https://github.com/rails/rails/blob/7f18ea14c893cb5c9f04d4fda9661126758332b5/railties%2Flib%2Frails%2Fcommands%2Fcommands_tasks.rb) 
calls the class *AppBuilder* inside the [*ActionMethods*](https://github.com/rails/rails/blob/7f18ea14c893cb5c9f04d4fda9661126758332b5/railties%2Flib%2Frails%2Fgenerators%2Frails%2Fapp%2Fapp_generator.rb) module.
  *  the third option is the name of your app. 
  *  We can also pass arguments to `rails new` to better configure the application to our needs. We can find these options by appending `--help` command at the end of
`rails new`. But some of the commands that I use most are:
  * `--database=postgresql` or `-d postgresql` will configure the application to connect to a [PostgreSQL](http://www.postgresql.org/) database rather than SQLite, Rails default database. This in turn creates and configures the `config/database.yml`file.
  * `--skip-turbolinks` I like skipping the installation for *turbolinks* as well because it sometimes have conflicts with my javascript. 
  * `--skip-test-unit` So why would I want to skip [Test::Unit](https://github.com/test-unit/test-unit)? Although I am a fan of it's simplicity and no overhead configuration, I may want to use Rspec as well, so I like the option of not having the `/test` created from the beginning.
  * Haml is my templating of choice, but I have not found an option for Rails to create `haml` files out of the box with `rails new` command. So after creating a base application, I add the `haml-rails` gem in my `Gemfile` and then can run `rake haml:erb2haml` from the command line to convert all the `erb` files to `haml`.

2. `rails server` or `rails s`
  * The `rails server` command launches a small web server named WEBrick which comes bundled with Ruby.
      This server listens to port 3000 by default, but that can be easily changed by passing `-p <port number>`

#### Rails Generators

1. `rails generate scaffold Post body:text title`
  * By running the scaffold command, Rails will generate all the code below *(model-migration, controller, views, tests)*
      Although when developing real world app I never use the scaffold generator. It creates too much unnecessary code. 

2. `rails g controller`
  * Controller names are plural.
  *  So when running `rails g controller <name>` we should make the `<name>` plural, or rails will complain. This command like the rails new command takes arguments as well. `rails g controller posts index show` will create 

              create    app/controllers/posts_controller.rb 
              route     get 'posts/show'
              route     get 'posts/index'
              invoke    erb
              create    app/views/posts
              create    app/views/posts/index.html.erb
              create    app/views/posts/show.html.erb
              invoke    test_unit
			  create    test/controllers/posts_controller_test.rb
              invoke    helper
		      create    app/helpers/posts_helper.rb
              invoke    test_unit
              invoke    assets
              invoke    coffee
              create    app/assets/javascripts/posts.coffee
              invoke    scss
              create    app/assets/stylesheets/posts.scss

    Note the created views and assets. 

3. `rails g model`
  * Models are singular.
  * The command above also takes options

            rails g model post body:text title
              invoke    active_record
              create    db/migrate/20160313045013_create_posts.rb
              create    app/models/post.rb
              invoke    test_unit
              create    test/models/post_test.rb
              create    test/fixtures/posts.yml

With this command Rails gives us not only the model for post, 
but also creates a migration for us *(we'll talk about migration in later posts)* 
Where we have the table name for *well you guessed it* posts, 
and the columns for `body` and `title`

  * For *Single Table Inheritence (STI)* we can also pass `--parent article` option. 
This would be it's generated code. 

        class Post < Article
        end 

  * `<model_name>:references` When you want to have a foreign key based on model *associations* we have the option to pass in a `reference`. This will add the `<model_name> id` to the table. 
  * `<model_name>:references{polymorphic}` for polymorphic associations.

#### Why is this important?

Using Rails built-in generators can help us to create applications faster and less error prone. It will create the correct directories/files based on its conventions. 
