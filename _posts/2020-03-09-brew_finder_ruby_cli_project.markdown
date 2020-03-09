---
layout: post
title:      "Brew Finder: Ruby CLI Project"
date:       2020-03-09 16:19:42 -0400
permalink:  brew_finder_ruby_cli_project
---


### So what's up with Brew Finder?

TL;DR? Check out the video of Brew Finder in action:

<iframe width="560" height="315" src="https://www.youtube.com/embed/UK6L221Ed-Y" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

I just hit a big milestone, the first final project for a language module in my Flatiron School journey! We were tasked with creating our own Ruby gem that would use a command line interface (CLI) to allow a user to interact with the program and access data (either from an API or scraped from a website).


This was the first time I was asked to create something of my own from scratch—until this point, we had been coding in test-driven labs. It was daunting to think about building something with no further guidance than the basic project requirements, so I decided to K.I.S.S. (keep it simple, stupid). Instead of coming up with a program and then finding the website or API that would allow the program to do what I wanted, I decided to start with finding an API that I knew I could work with and would be relatively simple. I was looking for free, easy access without a key if possible, and simple output. I found the perfect API in the [Open Brewery DB](https://www.openbrewerydb.org/): it has a number of easy calls to get different arrays of results, and the results are always an array of hashes and no further nested data structure beyond the tag list array. Sweet.

![](https://i.imgur.com/FRnP386.png?1)

Now I had an API that would be easy to work with, I wanted to build the simplest possible CLI that would meet the requirements of the project. I wanted a program that would:
1. Welcome user and ask for zip
2. Return a list of breweries from that zip
3. Ask user to pick a number from the list
4. Display details about the selected list item
5. Give the user the option to pick another from list, search again, or exit

### The Coding Process

So now I was had an API and knew the basic functionality of my program... and I had no idea how to start! The blank page is scary, yo.

![](https://i.imgur.com/81gyRqk.png?1)

I reviewed the project requirements once again, and found a super helpful resource on the information page: a video of Flatiron School founder, Avi Flombaum, talking through the process of building a CLI gem (called Daily Deal) from the ground up. I was able to follow Avi’s process and get my own project up and running relatively easily:
1. **Imagine the gem:** what is the user experience?
2. **Start with project structure (i.e., files):** Google the structure and use tools like Bundler to make set up a breeze.
3. **Start with entry point:** how does the user execute the program?
4. **Force the run file to create the CLI** (and subsequently every other object).
5. **Stub out interface:** make fake objects to get things working together.
6. **Make things real:** start using real data to make objects.
7. **Discover objects:** Each object does one thing, each method has one function.
8. **Red, Green, Refactor:** Get something working, then break it and make it better.

I had already done step 1 by finding my API and sketching out the basic funtion of my CLI, so next was setting up the file structure. Bundler is fantastic for setting up the basic gem structure and providing the environment file, version, etc. I did a little renaming and restructuring to make it look the way I wanted, but Bundler did most of the heavy lifting. On to step 3, how the program is executed in the bin folder:

```
#!/usr/bin/env ruby

require "bundler/setup"
require "env"

BrewFinder::CLI.new.call
```

I hadn't made the CLI class yet, or the call method, but I knew I would need to set Ruby as the environment, require the Bundler set up and my environment folder, and I wanted the program to run as soon as a new instance of the CLI class was instantiated. By instantiating the CLI and the `.call` method, I was "forcing the run file to create the CLI."

Next I was just creating basic objects and trying to get them to play nicely together. I made a CLI that put out simple messages and made a Brewery class and populated it with objects that were hard coded. Once I got the CLI working with the Brewery class, I was ready to use the API to create real Brewery objects. My Brewery and API objects were pretty simple and easy to get functioning properly. For the Brewery object, I gave it all the attributes from the Open Brewery DB results, used a hash and the `.send` method to assign the key-value pairs to the attributes, made a display details method, and methods to access all Brewery objects and clear all Brewery objects.

```
class BrewFinder::Brewery
  attr_accessor :id, :name, :street, :brewery_type, :city, :state, :postal_code, :country, :longitude, :latitude, :phone, :website_url, :updated_at, :tag_list
  
  @@all = []
  
  def initialize(hash)
    hash.each {|k, v| self.send("#{k}=", v)}
    @@all << self
  end
  
  def self.display_details(index)
    b = self.all[index]
    puts "---------------"
    puts "#{b.name} is located at #{b.street} in #{b.city}, #{b.state}."
    puts "Here are some details about #{b.name}:"
    puts "Type: #{b.brewery_type}"
    puts "Phone: #{b.phone}"
    puts "Website: #{b.website_url}"
    puts "---------------"
  end
  
  def self.all
    @@all
  end
  
  def self.destroy_all
    @@all.clear
  end
    
end
```

For the API, I just needed to call for breweries by zip code and use that returned array of hashes to instantiate new Brewery objects. The method takes a zip code as an argument and interpolates the zip code into the query to Open Brewery DB.

```
class BrewFinder::API
  
  def self.breweries_zip(zip)
    breweries = HTTParty.get("https://api.openbrewerydb.org/breweries?by_postal=#{zip}")
    
    breweries.each {|brewery_hash| BrewFinder::Brewery.new(brewery_hash)}
  end

  
end
```

The CLI was where the magic happens. I got it working with just the zip code really quickly, but then I need to spend some time "discovering objects" and methods. I got it to the point where I felt each object and method did one job, but I still wanted more so I decided to expand the scope of my project.


### Shining it up

Once I had the zip code search working (meaning I was able to start the program, run the search, look at results, and search another zip code without exiting the program), I was getting some momentum in my coding and wanted to add some extra bells and whistles. I added a search by state method to the API class:

```
  def self.breweries_state(state)
    breweries = HTTParty.get("https://api.openbrewerydb.org/breweries?per_page=50&by_state=#{state}")
    
    breweries.each {|brewery_hash| BrewFinder::Brewery.new(brewery_hash)}
  end

```
	
The Brewery object didn't need any changes, it was still doing its job of instantiating objects, keeping track of the collection and clearing the collection between searches thanks to the CLI calling on the `.destroy_all` method.

I edited the CLI to take either zip or state searches, display results differently depending on the type of search (display the street for zip searches and the city for state searches), and be able to handle invalid input and loop users back to the search function. I changed the placement of `while` loops and phrasing of prompts to make sense in the various scenarios where they might appear and ran my program dozens and dozens of times trying every combination of valid and invalid entries I could think of. I was in the Red, Green, Refactor step and it was actually really fun! Finally I added some color using the colorize gem and ASCII art that I got from [this cool site](https://asciiart.website/index.php?art=food%20and%20drink/beer) and then edited the art to make it my own by changing the handle, letters on the mug, and added color. 

```
class BrewFinder::CLI 
  
  def call 
    welcome
    search_breweries
    brewery_details
  end
  
  def welcome 
    puts "Welcome to Brew Finder! Let's find you some brews near you.".colorize(:yellow)
    puts
   puts "              ~  ~"
   puts "         ( o )o)"
   puts "       ( o )o )o)"
   puts "     (o( ~~~~~~~~o "
   puts "     ( )'" + " ~~~~~~~' ".colorize(:yellow)
   puts "     ( )|) " + "      |-.__.. ".colorize(:yellow)
   puts "       o" + "|   _    |-__  | ".colorize(:yellow)
   puts "       o" + "|  |_)   |   | | ".colorize(:yellow)
   puts "        |  |_)   |   | | ".colorize(:yellow)
   puts "       o" + "|        |  / /  ".colorize(:yellow)
   puts "        |        | / / ".colorize(:yellow)
   puts "        |        |- ' ".colorize(:yellow)
   puts "        .========.  ".colorize(:yellow)
   puts ""
  end
  
  def search_breweries
    puts "Would you like to see a selection of 50 breweries in your state,".colorize(:yellow)
    puts "or all the breweries in your zip code? Please type 'zip' or 'state'.".colorize(:yellow)
    choice = gets.strip.downcase
    
    if choice == "zip"
      puts "Please enter your 5-digit zipcode.".colorize(:yellow)
      input = gets.strip.to_i
      BrewFinder::API.breweries_zip(input)
      display_zip
    elsif choice == "state"
      puts "Please enter the full, unabbreviated name of your state.".colorize(:yellow)
      input = gets.strip.downcase.gsub(" ","_")
      BrewFinder::API.breweries_state(input)
      display_state
    else
      puts "I'm sorry, that is not a valid choice.".colorize(:yellow)
    end
  end
    
  def display_zip
    puts ""
    BrewFinder::Brewery.all.each.with_index(1) {|b, i| puts "#{i})".colorize(:yellow) + " #{b.name} - #{b.street} - #{b.brewery_type}"}
    puts ""
    puts "Which brewery would you like to learn about? Please enter a number.".colorize(:yellow)
  end
  
  def display_state
    puts ""
    BrewFinder::Brewery.all.each.with_index(1) {|b, i| puts "#{i})".colorize(:yellow) + " #{b.name} - #{b.city} - #{b.brewery_type}"}
    puts ""
    puts "Which brewery would you like to learn about? Please enter a number.".colorize(:yellow)
  end
  
  def brewery_details
    input = nil 
    while input != "exit" 
    puts "You can type 'start over' to search again or 'exit'.".colorize(:yellow)
  
    input = gets.strip.downcase
      if input == "exit"
        goodbye
      elsif input.to_i > 0 && input.to_i <= BrewFinder::Brewery.all.length
        BrewFinder::Brewery.display_details(input.to_i-1)
        puts "Pick a new number from the list to learn about another brewery.".colorize(:yellow)
      elsif input == "start over"
        BrewFinder::Brewery.destroy_all
        search_breweries
      else
        puts "Not sure what you meant... Please pick a number from the list.".colorize(:yellow)
      end
     end
  end
  
  def goodbye
    BrewFinder::Brewery.destroy_all
    puts "Goodbye, and thanks for checking out Brew Finder!".colorize(:yellow)
  end
  
end
```

My beer mug in all its colorized glory:

<a href="https://imgur.com/DVf4zV6"><img src="https://i.imgur.com/DVf4zV6.png" title="source: imgur.com" /></a>


### Lessons Learned

What surprised me the most about this project was how much FUN I had! Up until this project the coding labs had been rewarding, but I wouldn't call them "fun." With this project, I couldn't get away from my computer—I'd tell myself I was done for the night, then 30 minutes later I'd be back trying to smooth out some wrinkles, hunting for bugs, looking for the DRY-est code I could get. As a relative newb in coding I'm sure there's room for improvement, but I'm really proud of my work on this project and think it does a pretty good job of following best practices (I was tempted to use `GOTO` when a loop was about to break my mind, but refrained and refactored...).

![](https://imgs.xkcd.com/comics/goto.png)


I also learned a lot more about how Ruby functions within the larger landscape of technology, rather than just learning the syntax and methods. By creating the file structure and environment, requiring relative files, installing gems, etc I feel like I have a much more holistic understanding of programming than I had been getting from labs up to this point.

I feel renewed energy in my learning journey, and even better I feel prepared and interested in starting side projects of my own to work on outside of Flatiron curriculum—I'm thinking I'll try another CLI gem but using scraped data rather than an API.

And lastly, check out my gem on [GitHub](https://github.com/LeonorPDX/brew_finder)!

