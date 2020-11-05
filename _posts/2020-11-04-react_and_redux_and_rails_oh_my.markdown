---
layout: post
title:      "React and Redux and Rails, oh my!"
date:       2020-11-04 14:51:19 -0500
permalink:  react_and_redux_and_rails_oh_my
---


I can't believe we're already here, at the end of my journey with Flatiron School! I just finished my final portfolio project: I got to build on my existing full-stack knowledge and get deeper into JavaScript with my newly acquired skills in the React and Redux libraries.

For this project, students were tasked with making a single-page web application that used React's declarative routing to render different "pages," all from a single HTML file. We were also required to make a backend API with Rails, manage state on the frontend with Redux, and use Thunk middleware to handle asynchronous fetch requests and dispatches to our reducer(s).

For my project I made a book browsing app where users can view an index of books, search by book title, and add or remove books from a personal collection. Users are also able to record notes on books that are in their collection, which are rendered on the book's show page. While a user enters a username when they first visit the app, I did not use any authorization/authentication/session management on the frontend—instead I used a simple workaround on the backend to simulate a user session experience. I'll get into that more later in this post.


Check out the [GitHub repo](https://github.com/LeonorPDX/react-books-app) to look at the code or clone it to try yourself, or watch the video to see Boss Books in action.

<iframe width="560" height="315" src="https://www.youtube.com/embed/-XKY1mlPLbU" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


## My Process

This project actually requires building two separate apps. They work together but are completely independent, running on separate servers. In order to have the two work well together, planning was key. I've found strong planning at the beginning of a project greatly increases my productivity and allows me to quickly work through the actual coding.

Like in my previous projects, I started with wireframing pages and writing user stories; this gave me a clear picture in my mind of what the user experience would look like. This also let me work out how I would need to structure state in the Redux store, how state would be used in different parts of the app, and what fetch requests I would need to make. Next I worked on listing out the models and associations on the backend, the controller actions I would need for each fetch, and the specific JSON I wanted returned to fetch requests to make manipulating state as easy as possible.

Then it's time to get coding! I started with the Rails API because it's quick and easy for me to set up at this point, and my API performance would dictate how my action creators, fetch requests, and dispatches to my reducer would work on the frontend. I used the `rails new` command with the `--api` flag to get the basic file structure set up, used `rails g resource` to make each of my models and the controllers I would need, then I did some quick validations and set up the associations between my models.

![](https://i.imgur.com/wFPvLjL.jpg)

Next I seeded the database. I wanted the app to have a decent index of books so the user would have many books to choose from; I plan on reworking this project and creating a rake task to periodically call to a "best sellers" API to add books to the database. But for this version, one good batch of seeds would work. I used the Rest Client gem and the Google Books API to quickly create 80 books with all the data needed for my Book model. Here's the JSON response from the Google Books API:

![](https://i.imgur.com/rhkIJw6.png)

And the code in my `seeds.rb` file to create Book model instances from the response:
```
require 'rest-client'
require 'json'

firstBatch = RestClient.get 'https://www.googleapis.com/books/v1/users/102281797701392507828/bookshelves/0/volumes?maxResults=40'
secondBatch = RestClient.get 'https://www.googleapis.com/books/v1/users/102281797701392507828/bookshelves/3/volumes?maxResults=40'

firstJSON = JSON.parse(firstBatch)
secondJSON = JSON.parse(secondBatch)

firstArray = firstJSON["items"]
secondArray = secondJSON["items"]

firstArray.each do |book|
    Book.create(title: book["volumeInfo"]["title"],
    subtitle: book["volumeInfo"]["subtitle"],
    authors: book["volumeInfo"]["authors"].join(", "),
    description: book["volumeInfo"]["description"],
    img: book["volumeInfo"]["imageLinks"]["thumbnail"])
end

secondArray.each do |book|
    Book.create(title: book["volumeInfo"]["title"],
    subtitle: book["volumeInfo"]["subtitle"],
    authors: book["volumeInfo"]["authors"].join(", "),
    description: book["volumeInfo"]["description"],
    img: book["volumeInfo"]["imageLinks"]["thumbnail"])
end
```

The maximum response from Google Books is 40 items, so I made two requests to different bookshelves, giving me 80 books for my seeds.

Next I made my controller actions and tested them in my browser so I could see the JSON response my Rails API was returning. I wanted to know exactly what I was working with on the frontend.

Then I was ready to dive into the frontend. I made the directory with `create-react-app`, added packages with yarn, set up my `index.js` file to set up the Redux store and Thunk middleware, and configure the React and Redux dev tools. I quickly got `App.js` and `BooksContainer.js` files set up, and made an action creator to fetch books from my Rails API, and a reducer to update the Redux store. With `console.log` and the dev tools in the browser, I could see my frontend was successfully getting information from the backend.

And after that, it was an iterative process of adding a component, examining its state or props to make sure information was getting passed through the component tree correctly, and incrementally adding functionality. 

My reducer file, using `combineReducers` from Redux to export a single root reducer:
```
import { combineReducers } from "redux";
 
const rootReducer = combineReducers({
  books: manageBooks,
  user: manageUser,
});
 
export default rootReducer;


function manageBooks(state = [], action) {
    switch (action.type) {
      case 'ADD_ALL_BOOKS':

        return action.books

      default:
        return state;
    }
};

function manageUser(state = {username: "", id: 0, userBooks: [], userNotes: []}, action) {
 
  switch (action.type) {
    case 'ADD_USER':

      return {...state, username: action.user.username, id: action.user.id, userBooks: action.user.books, userNotes: action.user.notes }

    case 'REMOVE_USER_BOOK':

      return {
        ...state,
        userNotes: state.userNotes,
        userBooks: state.userBooks.filter(b => b.id !== action.book.id)
      }         

    case 'ADD_USER_BOOK':
          
      return {
        ...state,
        userNotes: state.userNotes,
        userBooks: state.userBooks.concat(action.book)
      } 

    case 'ADD_NOTE':
    
      return {
        ...state,
        userBooks: state.userBooks,
        userNotes: state.userNotes.concat(action.note)
      } 
      

    default:
      return state;
  }
};
```

My `App.js` file, setting up the main switch for my routes:
```
import React from 'react';
import { Route, Switch } from 'react-router-dom';
import { connect } from 'react-redux';

import BooksContainer from './containers/BooksContainer';
import UsersContainer from './containers/UsersContainer';
import NavBar from './NavBar';
import Welcome from './components/users/Welcome';

import { fetchUser } from './actions/fetchUser'


class App extends React.Component {

  componentDidMount() {
    this.props.fetchUser()
  }

  render() {

    return (
      <div className="App">
        <NavBar userId={this.props.userId} />

        <Switch>
          <Route path='/books' render={(routerProps) => <BooksContainer {...routerProps} />} />
          <Route path='/users/:id/books' component={UsersContainer} />
          <Route exact path='/' component={Welcome} />
        </Switch>
      </div>
    );
  }
}

const mapState = state => {
  return {
    userId: state.user.id
  }
}


export default connect(mapState, { fetchUser })(App)
```

## Challenges

The biggest challenge I faced was losing the user information from my state on page refreshes. As long as a user uses the links to navigate, the `Links` imported from `react-router-dom ` keep the Redux store intact and everything works. However, if a user refreshes, or manually types in a URL, the user state was being lost. The books were being repopulated because the books are fetched within a life-cycle hook `componentDidMount`, so a refresh just causes another fetch and the store is created again when the results from that fetch hit the reducer. 

In my initial app design, however, the user was fetched when a user submits the sign-in form; the problem was that my design required user input and form submission to get the correct user, so how do I restore the state in the event of a refresh?

The answer was I needed another life-cycle hook, but I needed a way to always retrieve the correct user. 

I was stumped for a day—I considered using local storage or something on the frontend, but I didn’t want to attempt to store anything user-related on the frontend without having proper protection of user data; even though it’s a “faked” login and only a username, it isn’t a habit I wanted to form. Then it hit me, just make a class variable in the User model on the backend and set it to the user’s ID when they sign in! Then I could add a `componentDidMount` to fetch the current user based on the class variable in the backend. Now any refreshes will still trigger a fetch for both books *and* the user, and we would be able to get the right user without any user input. While not conventional, it works for the sake of simulating a user sign-in experience.

In the future, I’d like to rebuild this project, or another React/Redux project, and incorporate [AuthO](https://auth0.com/docs/libraries/auth0-react); when handling user data, it’s best to use a tool that is tried and tested and has the resources of a dedicated team of developers debugging and optimizing security—it will almost certainly be much better than anything an individual programmer could create on their own.

## Successes

I am getting so much faster and better at debugging! Knowing when to use `byebug` or dip into the Rails console on the backend, using `debugger` on the frontend to pause processes and closely examine how state and props are changing, using `console.log` to see how props are getting populated as components mount (asynchronously, duh), being able to check out the component tree in React dev tools and inspecting the props on various components, and using the Redux dev tools to monitor state and see when reducer actions are being triggered… There are so many resources at a developer’s disposal, and I feel confident in using a wide variety of debugging tools.

Building this project, I was able to quickly figure out why something was or was not working, and identify where I needed to fix the problem. Just a few months ago I would not have been able to figure out these complex problems on my own, knowing exactly where to look and how to find what I’m looking for. I’ve grown so much in such a short period of time, I can’t wait to see where my skills will be at a year from now!

## Closing Thoughts

This is my final project as a Flatiron student, but it is just the beginning of my journey as a programmer. I am so grateful I made the decision to learn software development, and I’m amazed at how much I’ve learned over the course of this program. I’m looking forward to a rewarding career filled with continued learning and problem solving, and in the meantime I’m excited to try my hand at building out some more personal projects and learning new languages—next up is C#, I’ll let you know how it goes 😊

But first, I’m going to take a dance party break and celebrate this achievement...

<iframe src="https://giphy.com/embed/l0MYt5jPR6QX5pnqM" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

Thanks for reading, and happy coding!




