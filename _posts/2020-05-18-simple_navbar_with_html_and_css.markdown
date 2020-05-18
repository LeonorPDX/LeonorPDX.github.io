---
layout: post
title:      "Simple NavBar with HTML and CSS"
date:       2020-05-18 20:32:21 +0000
permalink:  simple_navbar_with_html_and_css
---

On my last project, I had to hunt around through a number of blogs and websites in order to find a very simple, clean way to code a navigation bar that only used HTML and CSS. In case anyone else out there is looking for an easy way to set up a navigation bar, here's the code I ended up writing and explanation of how it works!

The end result of this code will look like this:

![](https://i.imgur.com/E0jKkkK.png)

This will be a horizontal bar with five buttons, and include a color change when the cursor hovers over a selection.

The basic HTML for the list is an unordered list of links wrapped in the `<nav>` tag. The `<nav>` element is useful for the CSS styling that will turn your unordered list into a navigation bar, but it also is important for user accessibility. User agents, like screen readers used by low-vision users, can use this element to filter content and make the website easier to navigate and remove extra "noise" by determining whether to omit the navigation-only content.

Your HTML will look something like this:
```
      <nav>
        <ul>
				
          <li><a href="/signup">Sign Up</a>
						</li>
          <li><a href="/login">Log In</a>
						</li>
          <li><a href="/about">About</a>
						</li>
          <li><a href="/contact">Contact</a>
						</li>
          <li><a href="/testimonials">Testimonials</a>
						</li>
       
        </ul>
      </nav>
```

Typically a navigation bar would live inside the `<header>` in your layout view. If you want to make your navigation bar dynamic so it changes based on whether a user is logged in or not or something like that, you can easily change the visible links by inserting some Ruby and using am `if`/`elsif` block. Just remember that the navigation bar is going to be styled for 5 links, so keep the number of list items the same.

Now for the CSS:

```
    nav {
      width: 100%;
      margin: 20px 0;
    }
```

Set the width of the navigation bar to 100%, so it's the same size as the rest of the body content.



```
    nav ul{
      width: 100%;
      list-style: none;
      margin: 0;
      padding: 0;
			overflow: hidden;
    }
  
    nav ul li{
      width: 20%;
      float: left;
    }
```

Set list style to none so you don't have any bullets. For the list items, we float them left so they appear in a row instead of a vertical list. We set the width to 20% because we have five items; if you wanted to have four items, for example, you could change the width for each list item to 25% (one-forth of the width). With floated children, the list items are pulled out of the page flow; because of this, the `<ul>` will think it has no children and collapses. To trick the parent element into recognizing its floated children, we declare `overflow: hidden`. Even if it looks OK on some browsers without declaring `overflow: hidden`, the float list can cause problems in other aspects of layout down the road.



```
    nav ul li a{
      text-decoration: none;
      color: rgb(0, 0, 0);
      text-align: center;
      padding: 8px 0;
      display: block;
      width: 100%;
      background-color: rgb(230, 215, 173);
    }

    nav ul li a:hover {
      background-color: rgb(252, 234, 184);
    }
```

For each link in the navigation bar's unordered list, you can set text decoration to none (removes the link underline), set the text color and style to whatever you like, and give it a background color. `display: block` makes the link fill the hight and width of the parent `<li>`. Depending on your reset styles, you may or may not have to also set the width to 100%, so it fills the entire width of the `<li>`.

Finally, set a contrasting background color for the link on hover. Anchor elements also have visited and active states, if you want to set those to a contrasting color as well.

And that's it, a really quick and simple way to make a navigation bar with just some basic HTML and CSS! 
