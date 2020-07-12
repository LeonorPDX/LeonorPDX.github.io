---
layout: post
title:      "The Hikers App: Rails Project"
date:       2020-07-12 13:40:01 -0400
permalink:  the_hikers_app_rails_project
---


I just finished another portfolio project for Flatiron School’s Online Software Engineering Program, and I’m super proud of this app—with each project, I can see my competency increasing by leaps and bounds and the projects I create looking more and more like production-quality apps.

The requirements for this project were pretty extensive, including a number of model association requirements, nested resources, validations, error message display, user direct sign up and sign up through third party provider, and using some class level ActiveRecord scope methods.

I made an app for hikers to find trailheads and hikes, as well as publish their own hikes. Users can check-in at trailheads and leave trip reports on hikes, and view other users’ check-ins and trip reports. Seeing other users’ check-ins at a trailhead lets the user get a feel for how popular a particular trailhead is and how crowded it might be. Trip reports on hikes gives users more information on current trail conditions, trail closures, and other information that might impact their hike.

To see the app in action, see my video walkthrough:


### My Process

My first step was to outline my models and their associations, and to make sure my project would meet the minimum requirements for model associations.

![](https://i.imgur.com/hahCNeO.png?1)

Once I had the models planned, I worked on user stories. I followed Atlassian's method of “persona + need + purpose” and wrote out what a first time visitor to the site would need and expect, what a regular user would need and experience on various pages, and the content that would be able to see on various views. I wrote about 20 user stories, and by the time I was done I felt like I had a really clear picture in my mind of how this app would look and work.

My first actual code was to generate the new app with Rails, then get started on user sign up and sessions. I used [Devise](https://github.com/heartcombo/devise), a fantastic gem for setting up everything you need for managing users and sessions. In another blog post I'll go into detail of how to configure Devise with Rails, but suffice it to say for about 10 minutes of configuration work you get the user model, all the session-related views, controller actions, and routes... it's GREAT.

Next I added [Omniauth](https://github.com/omniauth/omniauth) to handle my third-party provider sign ups. I had to override Devise's controllers in order to set my callbacks and the registrations controller to use the params I wanted for my user model, but overall it is a quick and easy set up. I added sign up through Facebook or GitHub.

The callbacks controller:
```
class CallbacksController < Devise::OmniauthCallbacksController
    def facebook
        @user = User.from_omniauth(request.env["omniauth.auth"])
        sign_in_and_redirect @user
    end

    def github
        @user = User.from_omniauth(request.env["omniauth.auth"])
        sign_in_and_redirect @user
    end
end
```

The route overriding Devise's callback controller in config/routes.rb:
```
devise_for :users, controllers: {registrations: "registrations", omniauth_callbacks: "callbacks"}
```

Once I had the user registration and sessions working smoothly. I was ready to dive into the project. I started with just the Hike and Trailhead models: first I used `rails generate model` to create the models, migrations, controllers, and view directories. I then added associations and validations in the models. I wrote a little seed data and played around with it in `rails console` to make sure my associations were all working correctly.

```
class Hike < ApplicationRecord
    belongs_to :user
    belongs_to :trailhead
    has_many :trip_reports

    validates :name, presence: true, uniqueness: true
    validates :difficulty, inclusion: { in: %w(Easy Moderate Difficult), message: "%{value} is not a valid difficulty selection." }
    validates :distance, presence: true
    validates :elevation_gain, presence: true
    validates :hike_type, presence: true
    validates :description, length: { in: 50..20000}
end
```

```
class Trailhead < ApplicationRecord
    has_many :hikes
    has_many :check_ins
    has_many :users, through: :check_ins

    validates :name, presence: true, uniqueness: true
    validates :location, presence: true, uniqueness: true
    validates :amenities, presence: true
    validates :fees, presence: true
end
```

Once the index and show views were working, I added controller actions and routes for Hikes to be a nest resource under Trailheads. Next I worked on forms for new Hikes and Trailheads. Rails form helpers make creating forms a breeze and I was able to get my controller actions and params all aligned with the forms very easily. The hike form has a hidden field to assign the trailhead ID if the new form is accessed through a nested route:

```
    <% if form_hike.trailhead.nil? %>
        <label class="label">Hike Trailhead:</label>
        <%= f.select :trailhead_id, options_from_collection_for_select(Trailhead.all, :id, :name) %><br>
        <i>Don't see your trailhead? Create a new trailhead <%= link_to "here.", new_trailhead_path %></i><br><br>
    <% else %>
        <%= hidden_field_tag "hike[trailhead_id]", form_hike.trailhead_id %>
    <% end %>
```

Now that I had the basics of the main models finished, I went through the same process for Check-Ins and Trip Reports. Check-ins appear on the Trailhead show page, and Trip Reports on the Hike show page. The forms and controllers for Check-Ins and Trip Reports were simpler than Hike and Trailhead.

Finally I added the ability to edit and delete Hikes and Trailheads, and then it was time to refactor. I moved the Hike and Trailhead forms into partials that I rendered with locals:

```
<%= render partial: "form", locals: {form_hike: @hike} %>
```

I also moved the lists of Hikes and Trailheads that appear on index pages, user show pages, and trailhead show pages into a partial that also used a local. I refactored displaying errors in the views to generate flash errors in the controller, and then used an `errors` partial to render the errors on forms.

Controller generating flash errors:
```
    def create
        @hike = Hike.new(hike_params)
        @hike.user_id = current_user.id

        if @hike.valid?
            @hike.save
            redirect_to hike_path(@hike)
        else
            flash[:errors] = @hike.errors.full_messages
            render :new
        end
    end
```

Partial to render errors:
```
<% if !!flash[:errors] %>
  <ul>
    <% flash[:errors].each do |e| %>
      <li><%= e %></li>
    <% end %>
  </ul>
<% end %>
```

And lastly I moved some methods I had written in the models that had to do with displaying data into helper modules, thus maintaining seperation of concerns.

And then I was done, I met all the specs and everything worked! But... it was pretty ugly. Enter [Bulma](https://bulma.io/).

Bulma is a free, open-source CSS framework that can quickly add some quality-looking styling to your apps. I used their hero banner, levels, columns and the awesome media object to make my app look for like an actual app people would use rather than a word doc on a web browser.

Hero banner, nav bar, footer and columns:
![](https://i.imgur.com/9nzNLNl.png?1)


The famous media object making my trip reports look legit:
![](https://i.imgur.com/ByHWKIr.png?1)


Hike show page:
![](https://i.imgur.com/lVpcmQH.png?1)

The last thing I added was an option to add images to hikes—everything looks better with photos! I added a column to the Hike model, a field on the hike form for the image url, and a snippet of code to display the image if the Hike object has an image url.


### Challenges

Rails, Devise, and Bulma all have a lot of “magic” (i.e., things that happen under the hood). One of my main challenges was getting under the hood to get the gems to work with Rails. For example, I spent way too much time trying to get Rails to let me set a custom class on the delete button so I could leverage Bulma's button styling. Finally I just gave up on the Rails `button_to` method:
```
<%= button_to "Delete Hike", hike_path(@hike), method: :delete %>
```

And wrote out the code myself so I could set the class on the button:
```
<%= form_tag(hike_path(@hike), method: "delete") do %>
    <input type="submit" value="Delete" class="button is-danger">
<% end %>
```

It's a good lesson learned; Rails has so many helpers and it can make it quick and easy to get code working, but sometimes it's not worth it to use every helper available to you. Instead just write out the code yourself so you can have more granular control.

### Successes

By working on the user experience first, I was able to keep the big picture in mind as the code grew. Starting simple (user registraions and index/show) and allowing it to grow organically helped me keep the work manageable and the errors easily resolved (i.e., I knew everything was working correctly until I added this particular line of code, so the error must be associated with that code somehow).

Styling with Bulma was a great learning experience for me, and I got much more comfortable with CSS and HTML through his project. The norms, patterns, and syntax are becoming more familiar and intuitive for me than when I first started using them.

### Closing Thoughts

There are some extension features I'd like to add (such as user profile photos, check boxes to filter by multiple hike features instead of a drop down and filter by single feature), but overall I'm very happy with how this project turned out.

Like my other portfolio projects, I found it really easy to put in the hours while working on the Hikers App! I happily spent hours working on my code, researching new methods, and adding extension features. The end result of a project I can take pride in makes it all so worthwhile.

If you want to take a look at the code, or clone it and try it on your own machine, check out [the Hikers App on GitHub](https://github.com/LeonorPDX/rails-hikers-app).

Thanks for reading, and happy coding!


