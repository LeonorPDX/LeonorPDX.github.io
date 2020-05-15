---
layout: post
title:      "Cookbook Community: Sinatra Project"
date:       2020-05-15 19:58:56 +0000
permalink:  cookbook_community_sinatra_project
---


### Look, I made a website!

![Cookbook Community logo with old-fashioned looking woman wearing an apron at a table with bowls and a cookbook](https://i.imgur.com/ZRLDmXm.png)

I just finished my second portfolio project for Flatiron School's Online Software Engineering Program, and I finally have something I can show my mom that she recognizes as a real, honest-to-goodness website. Woohoo!

The project requirements were to create a MVC (model, view, controller) web app with Sinatra and ActiveRecord that uses sessions to keep track of users as they log in and out, and each user has full CRUD (create, read, update, destroy) abilities with resources that belong to that user. Additionally, the app should follow RESTful route conventions and separation of concerns.

I made a recipe sharing site where users can contribute recipes to a shared index of all recipes, update or delete their own recipes, and also create a collection of saved recipes in their own "Cookbook." Users can browse recipes by type, view recipes by author, and view other users' Cookbooks. 

I generated the basic directory structure and environment with [Corneal](https://thebrianemory.github.io/corneal/), which made getting the project started super quick and easy. I had to do some tidying up as I deleted gems I wouldn't use, added some I would, and removed folders that weren't relevant to my app (no javascript, no need for a separate lib directory, etc). 

The basic logic of the routes and sessions was largely predicated by various Flatiron School labs that built up to this project over the past months. However, the associations between the models and user ability to collect recipes was something new. I was happy that I was able to conceptualize the relationships between the models easily, and when I created the associations through ActiveRecord and gave a quick test in Tux everything worked exactly how I wanted it to. It felt like I really had a solid grasp of how ActiveRecord's associations work and the underlying SQL queries.

Finally, I made the site a little easier on the eyes and easier for the user to navigate. I created the styling and a dynamic nav bar with basic HTML and CSS picked up from various blogs and [W3Schools](https://www.w3schools.com/). I used [Canva](https://www.canva.com/) to make a logo with an image from [Pixababy](https://pixabay.com/) that was licensed for noncommercial reuse. 

To see the app in action, you can fork and clone the app from the [GitHub repository](https://github.com/LeonorPDX/sinatra-crud-cookbook-community) to your local environment and run shotgun to try it out, or you can check out this video walkthrough.

## [EMBED VIDEO]


### Models

![Models association diagram, as described below](https://i.imgur.com/qa0Frfr.jpg?1)

The minimum requirements for this project were that the app must have at least two models with one `has_many` and `belongs_to` association. Cookbook Community has a User model that `has_many` Recipes, and each Recipe `belongs_to` the User that created it. I added a third model, SavedRecipes, that allows the User to also `has_many` Recipes *through* SavedRecipes. Recipes also `has_many` Users through SavedRecipes.

The only model that contains any code outside of associations and ActiveRecord validations is the User model. There are two methods, `slug` and `find_by_slug` that make the username URL-friendly so it can be used in the route to the user's show page.

### Views

The views are organized in folders that relate to the controllers: sessions, recipes, and users. There is also a homepage view that lives in the root of the views subdirectory; the homepage is a first landing page for the app and functions as the "About" page.

There is also a layout view that holds the header with logo and nav bar, some script that allows the frame to fit the content of each page, and the footer. Each of the other views is inserted into the layout view with the `<%= yield %>` keyword. 

### Controllers

Cookbook Community has four controllers: application, sessions, recipes, and users.

#### Application Controller

```
class ApplicationController < Sinatra::Base

  configure do
    set :public_folder, 'public'
    set :views, 'app/views'
    enable :sessions
    use Rack::Flash, :sweep => true
    set :session_secret, "cookbook_community_security"
  end

  get '/' do
    erb :homepage
  end


helpers do
  def logged_in?
		!!session[:user_id]
	end

	def current_user
		User.find_by(id: session[:user_id])
  end
  
  def require_login
    unless logged_in?
      redirect "/login"
    end
  end
end

end
```

The application controller inherits from Sinatra Base, giving the app access to all the functionality of the Sinatra DSL. It also sets the controller configuration, enables sessions and sets a session secret to encrypt the stored session data, and uses Rack Flash so we can show error messages to the user.

The application controller has the route to homepage and the helper methods. These helpers are all related to managing sessions and the user's status as logged-in or logged-out; using these methods in conjunction with conditional blocks controls whether users see certain buttons and links such as log in, log out, and CRUD actions with recipes.

All other controllers inherit from the application controller, so the configuration and helpers are available throughout the controllers.

#### Sessions Controller
The sessions controller handles everything related to setting and clearing the session data, so it has the routes related to signing up, logging in, and logging out. 

The post routes for signing and logging in have flash messages for the user if their request was not successfully completed. The sign up route uses ActiveRecord's error message to tell the user why their sign up attempt was not completed (e.g., username already in use, etc).
```
    post '/signup' do
        user = User.new(params)
        
        if user.save
            session[:user_id] = user.id
            redirect "/recipes"
        else
            user.errors.full_messages.each {|m| flash[:message] = m}
            redirect "/signup"
        end
    end
```

#### Recipes Controller
The recipes controller has the 7 basic CRUD routes related to recipes, plus one extra to show recipes by type:
```
    get '/recipes/type/:tag' do
        @recipes = Recipe.all.select { |r| r.type_tag == params[:tag] }
        @recipes = @recipes.sort_by{ |r| r.name }
        erb :"recipes/index"
    end
```

#### Users Controller
The users controller holds routes related to users' collections of recipes—the collection of recipes created by the user, and the user's Cookbook of saved recipes.

#### Lessons Learned on Controllers
In order to keep my code DRY, I wanted to streamline my redirect actions if a user is not logged in. The idea is that if a user is not logged in, they shouldn't be able to view any pages except the homepage and sessions pages. Sinatra has a before filter, `before do` to tell a controller to always do something before loading a route. I used this function with one of my helper methods at the top of the recipes and users controllers:
```
    before do
        require_login
    end
```
That way any user that tried to visit a recipe or user page would get redirected, and I wouldn't have to repeat the same line of code in every route. But when I used this, I found it was affecting my *sessions* controller. It worked at redirecting if the user is not logged in and tried to view a recipe, but if the user was logged in and tried to log out they would get a 302 redirect error as it looped over and over redirecting to login—what's going on there? Why is a filter in the recipes controller trying to redirect a sessions route?

What I learned is Sinatra has some slightly wonky behavior when it comes to mounting controllers, and the order in which they are listed in the config.ru file matters. In order to fix this bug, I had to make sure that the controllers with `before do` are listed after the controller that should not have the `before do`. Once I got my routes properly separated by concern and the controllers listed in the correct order in config.ru, the bug was fixed.
```
use SessionsController
use RecipesController
use UsersController
run ApplicationController
```

### App in Action

When a user first visits the app and signs up for an account, the signup form's HTML includes required patterns; this restricts the permitted characters in the username and gives the user helpful prompts if they attempt to sign up with invalid credentials. As mentioned above, the ActiveRecord validations handle checking the user's input against the database and returns an error through flash message if the username or email address is already in use.

When the user successfully signs up, they are automatically logged in and redirected to the all recipes index. The nav bar also changes once they are logged in to include useful links for navigating the app. 

The index recipes (and all lists of recipes) are listed alphabetically, and there are links at the top of the index to browse recipes by type. When viewing individual recipes, the visible buttons change based on if the recipe belongs to the current user and if the recipe is in the current users cookbook.

In addition to the views rendering buttons conditionally, there are redundant validations in the controller. The buttons don't appear if a user doesn't have permission to take that action, but also the controller checks if the recipe belongs to the user and doesn't render the view if the user doesn't have permission. This helps protect the recipes in case an enterprising user notices the URL patterns and attempts to type in the route to edit or delete a recipe they do not own.

### Closing Thoughts

For a basic web app, I'm really happy with how Cookbook Community functions. There are features I would have liked to add such as a password confirmation field on the signup form and a *"Are you sure?"* message to appear when a user deletes a recipe; however, these features are best achieved using JavaScript which I'm still learning about. Looking back at my earlier attempts at making HTML web pages, this app is miles ahead of what I was able to create just a few months ago! It's so satisfying to be learning something new and creating my own projects, and I'm excited to dive into Rails next.
