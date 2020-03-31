---
layout: post
title:      "Ruby on Rails: A First Project"
date:       2020-03-31 22:00:20 +0000
permalink:  ruby_on_rails_a_first_project
---


If you've stumbled across this blog post, it is quite likely that you are a Flatiron School student and are looking for examples of projects and/or blog posts. This particular post is intended to provided some helpful hints and resources to aid you in meeting the projects requirements.

Friendly Reminder: it is normal to feel overwhelmed and/or anxious as you approach this and any project. You are not alone in this feeling, and it behooves you to reach out to classmates, your cohort lead, and/or your educational coach if you should ever feel this way. The Flatiron and general dev community is very supportive!

Start With An Idea
"Scratch your own itch"- Tim Ferriss
When thinking about what project you would like to create, the best way to start is to ask yourself what you care about, and what application you wish you yourself had. Code is supposed to solve real-world problems of all sizes. Dig deep, and remember that the more you care about your project, and the more fun you can have building it, the better it's going to turn out.

Requirements
Moving on from Sinatra and on to Rails meant the requirements list grew to be quite extensive. Rather than bore you with all of the requirements, I want to point out some of the more challenging ones.

1. You must include at least one class level ActiveRecord scope method.
Must be chainable
Must use ActiveRecord Query methods
So, what is a scope?

A scope is just a method. We can call it on a model or an association, and we can chain it. It should return an ActiveRecord::Relation. You can read about them here:


https://guides.rubyonrails.org/active_record_querying.html#scopes

Scopes help us keep our controllers and views thin, which is why they're so important.

Here's a simple example, which exists within my Donations model.

scope :recent, -> { order(date: :desc) }
Having this in place allows me to call :recent on an association, like so:

current_user.donations.recent.each do |donation|
TO BE CLEAR- this is a very simple use of scope, and you may need something more complex. Think about what makes sense for your application, and build that.

2. Your application must provide standard user authentication, including signup, login, logout, and passwords.
Do you remember how tedious setting up user authentication was in Sinatra? Not only is it tedious, but it's also quite possible to miss crucial validations. Skilled developers knew this, and so created the Devise gem to handle authentication, so that you don't have to.

To learn about Devise and how to use it, please visit:

https://github.com/heartcombo/devise#getting-started

3. You must include and make use of a nested resource with the appropriate RESTful URLs.
Alright, this one can be challenging, but it's also quite a lot of fun.

Check out these routes:
resources :user do
    resources :donations, only: [:index, :new, :create, :show]
      resources :organizations, only: [:index, :show]
  end
Okay, yes, Cristal... what about them?

Well, first... We get access to really neat routes. For example, instead of simply having access to

new_donation_path

we get

new_user_donation_path

& creating a link to that page, gives us this:

link_to "Log a Donation", new_user_donation_path(current_user)...

Clicking on that link will take us to:

http://localhost:3000/user/3/donations/new

Notice that in order to get to the path, we need to give it an argument of current_user (which is a Devise built in helper method).

Similarly, upon creation of the donation in the create action, we want to redirect to that user's newly created donation. So what do we have to do to take that route?
  def create
    @donation = current_user.donations.build(donation_params)
    if @donation.save
      redirect_to user_donation_path(@donation.user, @donation)
    else
      render :new
    end
  end
We have to hand the user and the donation to the view!

@donation.user, @donation

@donation.user will provide the :id, while @donation hands the actual donation information.

NOTE: IF YOU HAVE A CUSTOM PATH (i.e. a non-RESTful action outside of index, new, create, show, edit, update, destroy), MAKE SURE TO DECLARE IT ABOVE YOUR NESTED ROUTES. Otherwise, you will not hit the action in your controller!
get '/donations/by_year, to: 'donations#by_year', as: 'by_year'`

for example, would have to be declared before the nested resources I've already laid out above.

Lastly...

4. Do not use scaffolding to build your project.
I repeat- do NOT use scaffolding in your project. It WILL be obvious- you will have unused folders and files that are generated upon running the scaffold generator. Your controllers will be littered with notes. I'm sure there are other giveaways I'm not aware of, but we can be sure the assessor has a keen eye for this.

Go forth and prosper!
I hope that some of the links and examples here are useful to you as you embark on building your first RoR project. Keep an eye out for a non-technical post which will be coming to you soon about the challenges of building my own project.

If you're having a hard time, just remember- you're not alone.

I myself wouldn't have made it without a little help from my friends <3

(&& by a little, I mean a-lot-tle.)

Happy coding!
