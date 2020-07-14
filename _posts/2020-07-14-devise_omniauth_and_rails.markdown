---
layout: post
title:      "Devise, Omniauth & Rails"
date:       2020-07-14 12:57:29 -0400
permalink:  devise_omniauth_and_rails
---

## User Authentication Made Easy

In this blog post, I’m going to give a quick tutorial on how to configure a new Rails app with two gems, Devise and Omniauth, to quickly get all aspects of your user authentication process off the ground. In less than 30 minutes, you can have an app ready for users to sign up, log in, and use their third-party provider account (like Facebook) to create an account on your app.

We will go over adding and setting up the required gems, customizing the User model that Devise generates, overriding Devise’s controllers so we can work with our customized User, and setting our method to make new Users from the third-party providers’ data.

We won’t be going into details about registering your app with third-party providers, but you can easily find up-to-date directions for whatever provider you’re working with by doing a quick Google search.

### Generate the App

From your terminal, use `rails new app_name`. Seriously that’s it, Rails is awesome. Don’t forget to `cd` into your new app directory and you probably want to also link up your new app to a version control system, like GitHub.

### Add Devise

According to the Devise gem README:
> Devise is a flexible authentication solution for Rails based on Warden. It:
> * Is Rack based;
> * Is a complete MVC solution based on Rails engines;
> * Allows you to have multiple models signed in at the same time;
> * Is based on a modularity concept: use only what you really need.

Basically it’s a really powerful gem, but it’s also modular so you can just use the parts you need without getting into all the features you don’t want right now (like mailers). The documentation is clear and ample, and there are tons of blogs about using Devise. Just Google any questions you have.

To get started, add `gem 'devise'` in your gemfile and run `bundle install` to get your gems and dependencies updated. Next, run `rails g devise:install`.

After running the Devise install generator, you’ll see a number of instructions in your console. Don’t worry about setting default URLs for modules you aren’t using, like mailers, but the rest are good tips. We are going to be customizing the views so go ahead and generate the Devise views. Set a root URL in `config/routes.rb` so you can check everything is working—I added `root application#home`, added the application controller method, and in the view put the following code (using Devise’s helper method, `user_signed_in?`) so I could easily see if I was able to log in correctly when testing it out on the server:

```
<% if user_signed_in? %>
	  <h1>User is LOGGED IN! :)</h1>
<% else %>
	  <h1>User is NOT logged in! :(</h1>
<% end %>
```

And it is handy to throw their suggested snippet of code in your layout to display the flash alerts that Devise creates:

```
    <p class="notice"><%= notice %></p>
    <p class="alert"><%= alert %></p>
```

Next, we’re going to create the User model though Devise. Devise doesn’t require you to call your authenticatable model “User,” you could call it Admin or whatever you want, but User is a standard choice. Run `rails g devise User`.

Now, we could run the migration right away, then add columns and tweak our User model, but I’m going to be sneaky and just add in a `name` attribute on our User model before we migrate. Open the migration that Devise just created, you will see code like this:

![](https://i.imgur.com/F5Qbsvu.png?1)

Devise creates a User model with an email and password, and some other stuff to make it recoverable, rememberable, and trackable. The User attributes needed for other modules are commented out. To add a name to our User, insert this line in the DeviseCreateUsers migration:

```
t.string :name, null: false, default: ""
```

You could add other attributes if you like, but don’t go overboard right now. You can always run other migrations to add columns later. For now, run `rails db:migrate`.

Take a moment to look at everything Devise has done for you: check out the views, look at `config/routes.rb`, take a quick gander at `config/initializers/devise.rb` (don’t worry, you don’t have to read every line), run `rake routes` to see the routes that Devise has configured, and check out the User model. You can also start the server (`rails s`) and look at the nice sign-up and log-in views that Devise created. Sweet!


### Overriding Devise Registrations

Next, we’re going to add a name field on our sign up form, update the params for the controller to accept the name input, and let Devise know we’re using our own controller in `config/routes.rb`. I also recommend validating the name and email attributes in the User model, but you do you with validations—I’m not getting into that in this blog post.

Open the registration form at `views/devise/registrations/new.html.erb`.

Add a field for the User name:

```
  <div class="field">
    <%= f.label :name %><br />
    <%= f.text_field :name, autofocus: true, autocomplete: "name" %>
  </div>
```

Also add the same field to the `edit.html.erb` form.

Next, make your own registrations controller that inherits from the Devise registrations controller and set up strong params that include name:

![](https://i.imgur.com/iaMBHrm.png)

And finally, let Devise know to use our customized registrations controller in `config/routes.rb`. Hey, while we’re at it, let’s make the login and logout routes a little prettier and easier to write out:

![](https://i.imgur.com/ajN8B7X.png?1)

If you run `rake routes` again you’ll see your login/signup/logout paths are updated. Try `rails s` and navigate to `https://localhost:3000/signup`, create an account, and you should get redirected to your home page. And now we have a working user authentication process! Thanks, Devise!


### Third-Party Providers with Omniauth

Most savvy web surfers expect to be able to log in to whatever app they want based on the login credentials from other apps, it’s pretty much the norm now to see the options to sign up with Google or Facebook. Omniauth is a gem that provides a library that standardizes the process for authenticating through third-parties, making it easier and faster to set up multiple providers.

To use Omniauth, add `gem 'omniauth-github'` and `gem 'omniauth-facebook'` to your gemfile and run `bundle install`. Most provider-specific Omniauth gems will require all the dependencies you need so it is not necessary to add `gem 'omniauth'` as well, but if you run into any difficulties with dependencies you can go ahead and add it to your gemfile.

Since we’re going to add Facebook and GitHub to our app, you will first need to register your app with these providers through their developer portals. To get to the developer page on Facebook just Google “Facebook for developers”, go to My Apps and click New App. For GitHub, go to Settings > Developer Settings > OAuth Apps and then create a new app from there.

The steps to register an app and the page set-up changes from time to time, so I won’t go into details about how to register your app here—Google it for up-to-date guides. Just fill in info as prompted and use localhost:3000 for your website URL. In the fields for “Valid OAuth Redirect URIs” or “Callback URL” enter `http://localhost:3000/users/auth/facebook/callback` or `http://localhost:3000/users/auth/github/callback` for Facebook and GitHub, respectively. And find the App ID/Client ID and key that they provide you after registering, you’ll need that to configure Omniauth in our next step.

In `config/initializers/devise.rb` around line 270 you’ll see the Omniauth section. Add the IDs and keys from Facebook and GitHub:

```
config.omniauth :github, 'PASTE ID HERE', 'PASTE KEY HERE'
config.omniauth :facebook, 'PASTE ID HERE', 'PASTE KEY HERE'
```

You have now told the third-party providers about your app by registering it with them, and told your app about its third-party providers by adding the ID and key to initializers. Now you just need to get the User model set up to create from third-party credentials, and set up the controller to use the new method you are going to write in your User model.

First we’ll add columns to the User model so a User can have a provider attribute and a UID attribute. UID is a unique user ID that will get us the hash of user info from the provider. In your terminal, run:

```
rails g migration AddOmniauthToUsers provider:string uid:string
```

Then run `rails db:migrate`.

Next go to your user model at `user.rb`. Make the model omniauthable (a Devise module) by adding it to the list at the top. Write a method to find or create from the provider data that accepts the authentication data as an argument:

![](https://i.imgur.com/Xq0XEhy.png?1)

The authentication data from the provider is a hash with “info” as a nested hash within it. We are extracting data from the nested hash by using `auth.info.SOME_OTHER_KEY`. We have the `if/else` statement to set the User’s name because Facebook has a `:name` key in their info hash, while GitHub has a `:username` key in their info hash. See an example of the auth hash below:

```
=> {"provider"=>"facebook",
"uid"=>"123456789012345",

"info"=>

 {"email"=>"user@example.com",
  "name"=>"User Example",
  "image"=>"http://graph.facebook.com/v2.6/123456789012345/picture"},

"credentials"=>

 {"token"=>"ABCDaEFbcGHIJKLMNdlOPeQRfSTUVWgXf1hYiZAjBkClDEmFG234n5oH6p7IJqKr0stLMNuOPQRv86S47TUVWX1YZwABCDxyz2EabcdFGeH4IfgJK9hLi0jM1kNOPQlRmn1oSTUp5qr7VWstuXvYZwxAByza807CbD9c3defEFGghijkHIJK",
  "expires_at"=>1503263133,
  "expires"=>true},

"extra"=>

 {"raw_info"=>
   {"name"=>"User Example",
    "email"=>"user@example.com",
    "id"=>"123456789012345"}}}
```

And last, we need to make a custom callbacks controller that overrides the Devise controller, just like we did with registrations. Make a controller and write methods for Facebook and GitHub:


![](https://i.imgur.com/h9oe9an.png)


Then add the callbacks controller in `config/routes.rb`:


![](https://i.imgur.com/5qzahCC.png?1)


Devise will magically (not really, it’s just code) add the “Log in with Facebook” and “Log in with GitHub” links to your views. Run `rails s` and check it out!


<iframe src="https://giphy.com/embed/s2qXK8wAvkHTO" width="480" height="316" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>


If you liked this walk-through please connect with me on [LinkedIn](https://www.linkedin.com/in/leonorcolbert/) and check out my code on [GitHub](https://github.com/LeonorPDX) &#128512;

Thanks for reading, and happy coding!

