---
layout: post
title:      "The Hikers App 2.0: JavaScript"
date:       2020-09-02 19:30:46 -0400
permalink:  the_hikers_app_2_0_javascript
---


Another portfolio project completed, another module of Flatiron School's Software Engineering program behind me! After developing a deep understanding of Ruby and Rails over the past months, it was so much fun to tackle something completely different with JavaScript; I was excited to finally be learning the language that makes responsive DOM styling possible, something users expect on pretty much every kind of professional website.

For this portfolio project, we were asked to create a single page web application that leverages our existing skills in Ruby and Rails to handle the backend, and uses our new skills in JavaScript to handle everything on the client-side—i.e., DOM manipulation such as rendering, updating, and handling form submissions. Using asynchronous JavaScript and JSON to communicate with the backend, our web application would allow users to perform at least two of the basic CRUD actions (create, read, update, destroy).

For my project, I opted to do a 2.0 version of my previous portfolio project, [the Hikers App](https://leonorpdx.github.io/the_hikers_app_rails_project). In this version, I decided to make a pared-down site that would include an index of all trailheads, nested indices of hikes at each of those trailheads, and a form for users to create a new hike.

<iframe width="560" height="315" src="https://www.youtube.com/embed/bD3BoH_NpSw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### My Process

The first step for me was sketching out a work plan, including user stories and wire frames of how I wanted my pages to look and work. I organized the work plan chronologically, highlighting when I should be moving back and forth between working on the frontend and the backend. Once I had decided on a plan of action, I was able to hit the ground running and start with building my backend.

The backend is based on Rails and Active Record to manage a database and communicate JSON to the frontend via RESTful routes. When generating the new app, I used the `--api` flag so Rails only create the files I would need for the backend. With my previous experience in Rails, I was able to get models, associations, routes, and controller actions configured very efficiently. Within 15 minutes of running `rails new`, I was in the console working with seed data and making sure everything on my backend was running smoothly. I then created the new frontend directory:

```
javascript-hikers-app/
 |- backend/
 |    |- app/
 |    |- (...other rails files and folders)
 |
 |- frontend/
 |    |- src/
 |        |- index.js
 |    |- index.html
 |
 |- README.md
 |- LICENSE
```

Next I started working with HTML, CSS, and JavaScript to get the frontend up and running. I used Bulma, the same CSS framework I used in my previous Rails project, so that the styling on my 2.0 version would be consistent. With Bulma, you are able to link to their CSS in the HTML without downloading and importing Bulma into your own CSS file; this worked great for me, but doesn't permit for customization of Bulma CSS (like color schemes, fonts, etc).

```
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.0/css/bulma.min.css">
```

My first step was to create an index HTML file and hard-code the format that I wanted for my trailheads index page; this helped give me a clear idea of how my JavaScript would need to relate to the HTML and what elements I would need to create for each trailhead and hike. 

```
<div class="title">
        <h2>All Hikes at Trailhead One</h2>
</div>

        <div class="tile is-ancestor">
            <div class="tile is-parent">
                <div class="tile is-child box">

                <div class="level">
                    <div class="level-left">
                        <div class="level-item">
                            <p class="title">Hike One</p>
                        </div>
                    </div>

                    <div class="level-right">
                        <div class="level-item">
                            <button class="button is-primary is-medium is-light">+</button>
                        </div>
                    </div>
                </div>


                <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin ornare magna eros, eu pellentesque tortor vestibulum ut. Maecenas non massa sem. Etiam finibus odio quis feugiat facilisis.</p>
                </div>
            </div>
        </div>
```

With Bulma's styling, the above HTML gave me a nice tile with a level title and button (code for the upper banner and buttons not shown):

![](https://imgur.com/UUqn2ei)

This was a crucial step for me, I know I would have been overwhelmed trying to write the JavaScript function to format each trailhead and hike tile had I not worked out how the finished HTML would need to look to get the formatting I wanted.

Next I wrote my first `fetch` request to the backend, worked on the Rails controller action to format the JSON with all the data I wanted, and slowly began building out the functionality of my frontend. For place-holders on event listeners, I used a lot of `console.log`'s so I could see which events and functions were being triggered as I tested the site. My project started all in a single JavaScript file, but I pretty quickly began separating my code into different files so I could maintain separation of concerns, and also make debugging and scanning the code much more reader-friendly. I also encapsulated the JavaScript in classes, allowing me to continuing thinking and working in an object oriented fashion. Object orientation has become second nature over the course of working in Ruby!

```
javascript-hikers-app/
 |- backend/
 |    |- app/
 |    |- (...other rails files and folders)
 |
 |- frontend/
 |    |- src/
 |        |- index.js
 |        |- appMain.js
 |        |- appAdapter.js
 |        |- trailhead.js
 |        |- hike.js
 |    |- index.html
 |
 |- README.md
 |- LICENSE
```

### Challenges

Context in JavaScript can be challenging, especially when dealing with asynchronous functions and promises. I handled this by making most of my functions static, allowing me to call them on the class from within other functions, classes, and contexts. I also used `bind` frequently, allowing me to retain access to the `this` context as I passed functions around in promises and callbacks.

### Successes

JavaScript had a steep learning curve, and a couple weeks into this module I was feeling pretty bewildered. I am happy that by the time I got to working on this project, JavaScript was feeling much more natural to me. One of the moments that made me feel the most confident in my coding was when I was coming towards the end of my project; the only thing left to do was to make the toggle buttons in the corner of my hike tiles work to show more or less information about the hikes.

I only had 15 minutes before I needed to leave for work, I wasn't sure if I should even start working on a new function when I probably wouldn't have time to finish it. I decided to give it a shot: I quickly figured out how I wanted to handle the button, wrote out a function and had it working on my browser in 10 minutes! I could believe that I was so comfortable working in JavaScript when just a few weeks before I had been totally stumped.

The toggle content function, in the Hike class:

```
    static toggleContent(event) {
        if (event.target.innerText === "+") {
            event.target.innerText = "-"
            let hikeId = event.target.id;
            hikeId = parseInt(hikeId, 0);
            const shortContent = document.getElementById(`hike-${hikeId}`);
            const longContent = document.createElement("div");
            const hike = Hike.all.find(e => e.id === hikeId);
            if (hike.imgUrl != "") {
                const img = document.createElement("img");
                img.src = hike.imgUrl;
                longContent.appendChild(img)
            }
            const description = document.createElement('p');
            description.innerText = hike.description
            longContent.appendChild(description);

            shortContent.appendChild(longContent);
        } else {
            event.target.innerText = "+"
            let hikeId = event.target.id;
            const shortContent = document.getElementById(`hike-${hikeId}`);
            const longContent = shortContent.lastChild;
            shortContent.removeChild(longContent)
        }
    }
```


### Closing Thoughts

As I enter the final module in Flatiron School's Software Engineering program, I'm feeling more confident and optimistic with each passing day. It was a difficult decision to transition into a new career, but I have found this work to be incredibly satisfying. The innovation and scope for continued learning throughout my career excites me, and I really enjoy seeing something I have imagined and created made real through programming. Sure, sometimes it feels like this...

<iframe src="https://giphy.com/embed/dlMIwDQAxXn1K" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

But when you're in the zone, it feels like this... 😎

<iframe src="https://giphy.com/embed/ZuhmYnikJOPPq" width="480" height="480" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

Keep the bugs coming, I love solving puzzles!

Check out [the Hikers App 2.0 on GitHub](https://github.com/LeonorPDX/javascript-hikers-app) to see the code and try it yourself. Thanks for reading!


