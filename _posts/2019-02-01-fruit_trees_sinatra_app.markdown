---
layout: post
title:      "Fruit Trees Sinatra App"
date:       2019-02-01 13:47:30 -0500
permalink:  fruit_trees_sinatra_app
---

Wow. It's taken me longer to get here than I would have imagined but I'm really pleased with this app. It may not be all that powerful or pretty but it does the job. I think in the future I might return to it and add some styling and functionality. Right now I need to just be happy with a project that delivers a minimum viable product and move on to bigger things in this course.

The app is a MVC-CRUD applications with two models, Homeowners and Trees. Homeowners have many Trees and a Tree belongs to a Homeowner. Homeowners must signup with a first and last name, email address and passowrd. Trees must be created with a variety, size and fruit weight. Fruit weight is the weight of the harvest fruit in pounds.

I made a bit of a stretch goal for myself to use a little bit of the Active Record(AR) validations avialable instead of creating my  own. I only did this for one model so I could demonstrate that I understood how to do it both with AR and in without. My Homeowner model looks like this: 

```
class Homeowner < ActiveRecord::Base
	has_secure_password
	validates :first_name, presence: true
	validates :last_name, presence: true
	validates :email, presence: true
	validates :email, uniqueness: true
	
	has_many :trees
end
```

With these AR validations I can mass-assign my Homeowner attributes instead of having to check if each attribute is filled in the form. 

```
post '/homeowners' do 
    **@homeowner = Homeowner.new(params)**
    if @homeowner.save
      session[:homeowner_id] = @homeowner.id
			...
```

vs

```
post '/trees' do
	  redirect_if_not_logged_in
	  if form_is_filled?
	    flash[:message] = "Your tree was successfully created"
	    **@tree = Tree.create(variety: params[:variety], size: params[:size], fruit_weight: params[:fruit_weight], homeowner_id: current_user.id)**
	    redirect "/trees/#{@tree.id}"
	...
	
	def form_is_filled?
		params[:variety] != "" && params[:size] != "" && params[:fruit_weight] != ""
	end
```

The pattern is a little different but with the AR validations requiring the presence for all my attributes I only need to check if I can save the object after instaniating a new one rather than having to check that the form is filled and then creating(which saves the object) an object with each param set manually. If you have a realy long form that could get very messy.

AR validations also come with specific error messages that you can use to improve user experience. If I try to create a new Homeowner without the required attributes I can see in tux that the database will rollback and not allow that Homeowner to be persistied.

```
>> homeowner = Homeowner.create
D, [2019-02-01T11:23:51.189457 #3820] DEBUG -- :    (0.1ms)  begin transaction
D, [2019-02-01T11:23:51.246680 #3820] DEBUG -- :   Homeowner Exists (0.2ms)  SELECT  1 AS one FROM "homeowners" WHERE "homeowners"."email" IS NULL LIMIT 1
D, [2019-02-01T11:23:51.247128 #3820] DEBUG -- :    (0.1ms)  rollback transaction
=> #<Homeowner id: nil, first_name: nil, last_name: nil, email: nil, password_digest: nil, created_at: nil, updated_at: nil>
```

In catching that rollback transaction, I can then check the errors on the instance I tried to create.

```
>> homeowner.errors
=> #<ActiveModel::Errors:0x007fc7bcd02588 
@base=#<Homeowner id: nil, first_name: nil, last_name: nil,
email: nil, password_digest: nil, created_at: nil, updated_at: nil>,
@messages={:password=>["can't be blank"],
:first_name=>["can't be blank"], 
:last_name=>["can't be blank"], 
:email=>["can't be blank"]}>
```

To expose just the messages and not the object I use the full_messages method

```
>> homeowner.errors.full_messages
=> ["Password can't be blank", 
"First name can't be blank", 
"Last name can't be blank", 
Email can't be blank"]
```

and then to use those with Flash I need to extract them from the array with the to_sentence method which gives me a string.

```
>> homeowner.errors.full_messages.to_sentence
=> "Password can't be blank, 
First name can't be blank, 
Last name can't be blank, 
and Email can't be blank"
```

I'm also validating the uniqueness of the email attribute so that a homeowner can't sign up twice with the same email. In this example I've already used the email kate@me.org for a homeowner.

```
>> homeowner.first_name = "bob"
=> "bob"
>> homeowner.last_name = "jones"
=> "jones"
>> homeowner.email = "kate@me.org"
=> "kate@me.org"
>> homeowner.password = "test"
=> "test"
>> homeowner.save
D, [2019-02-01T11:34:59.902993 #3820] DEBUG -- :    (0.2ms)  begin transaction
D, [2019-02-01T11:34:59.905018 #3820] DEBUG -- :   Homeowner Exists (0.3ms)  SELECT  1 AS one FROM "homeowners" WHERE "homeowners"."email" = 'kate@me.org' LIMIT 1
D, [2019-02-01T11:34:59.907237 #3820] DEBUG -- :    (0.1ms)  rollback transaction
=> false
>> homeowner.errors.full_messages.to_sentence
=> "Email has already been taken"
```

I can then use these specific messages to create flash messages for useres using interpolation. 
```
 flash[:errors] = "Account creation failure: #{@homeowner.errors.full_messages.to_sentence}"
      redirect '/signup'
```

There is a lot more to Active Record validations but I am pretty excited to see what they can with future projects. If you want to check out more about them I would start [here!](https://guides.rubyonrails.org/active_record_validations.html)





