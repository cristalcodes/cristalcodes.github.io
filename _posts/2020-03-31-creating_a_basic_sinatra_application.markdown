---
layout: post
title:      "Creating a Basic Sinatra Application"
date:       2020-03-31 21:59:25 +0000
permalink:  creating_a_basic_sinatra_application
---


If you've stumbled across this blog post, it is quite likely that you are a Flatiron School student and are looking for examples of projects and/or blog posts. This particular post is intended to provided some helpful hints and resources to aid you in meeting the projects requirements.

Friendly Reminder: it is normal to feel overwhelmed and/or anxious as you approach this and any project. You are not alone in this feeling, and it behooves you to reach out to classmates, your cohort lead, and/or your educational coach if you should ever feel this way. The Flatiron and general dev community is very supportive!

The Project
The goal of this project is to build a basic Sinatra application by utilizing the Sinatra framework in conjunction with ActiveRecord and SQLite 3.

Requirements
•The application should have 2+ models.
•Application has user accounts with unique login attribute
•Resource must have CRUD routes
•Ensures that users can't modify content created by other users
•Includes user input validations

Helpful Tips:
Start with the Corneal gem.

File dependencies can quickly become more terrifying than the project itself. Save yourself some hardship by installing the Corneal gem, which sets up the file structure for a Sinatra MVC application for you.

Create a project directory on your computer, then cd into it from your terminal. Once there, execute the following:
gem install corneal
You may be thinking, "I need to practice setting up file dependencies." I agree. However, there will be plenty of time to do so. Pay attention to the way the gem sets up the file structure, and study the file dependencies to try and understand them. It is an excellent model to be able to refer to in the future.

Keep It Simple, Silly

I know it's tempting to go all out with your project, but start simple and scaffold from there. Start with as few models as possible, and make sure you understand the relationships between them. You are required to have a has_many model and a belongs_to model.

Set up your models, controllers, and migrations as soon as you begin to work on your project. Things to keep in mind:

Controllers inherit from the Application Controller
ApplicationController inherits from Sinatra::Base
Models inherit from ActiveRecord::Base
Set up your config.ru file to run only one controller- the application controller. All other controllers are simply preceded by the word 'use'.
Make sure your config.ru file includes Rack::MethodOverride so that you are able to use this middleware for patching and deleting.
Validations Are Essential

If you've been reading your ActiveRecord Documentation (and pay attention if you haven't), you likely already know that ActiveRecord has built in validation methods. INCLUDE THESE! They belong to your models- not your controllers. Make sure to place them in the appropriate place!

Some validation methods provided by ActiveRecord that you may find useful:

inclusion
length
numericality
presence
uniqueness
See the ActiveRecord documentation for all available validations and use examples.

And for email validation, which is a little trickier, you'll be pleased to know that Ruby itself has a method for achieving the task. I've included it here for your convenience.

validates :email, format: { with: URI::MailTo::EMAIL_REGEXP }

Protect the User

Some key things to remember as you are building your application:

Make sure you have required the 'bcrypt' gem in your gemfile. This provides you with the has_secure_password macro.
The above macro requires that your users table include a column with a password_digest key.
In your user form, set up input types for password as "password" and not "text". This will protect your users password from appearing as plain text, and instead appears as bullets.
Remember to enable sessions and set your session secret.
In each view/edit/patch/delete method, protect your user by always checking if :
user is logged in (build helper methods in your application contoller)
the user is the current user
Test your application to ensure that you have properly coded the above. You can do so by downloading (if you haven't already) DB Browser (free) and then attempting to hack your own application by opening the developer console in your browser.
Closing Remarks
The best way to approach this project is with an open mind and a clear understanding of what you are trying to achieve. I highly recommend watching Avi's videos on the topic, as well as those of your cohort's lead to get your footing.

I hope that the information I have provided here will be helpful as you embark on this portion of your journey.

Please feel free to reach out by dropping a comment. Questions, concerns, and suggestions are all welcome here.

Happy Coding!
