---
layout: post
title:      "Web Accessibility: Part 4"
date:       2021-04-11 15:45:54 -0400
permalink:  web_accessibility_part_4
---


Here it is, our final installment in the Web Accessibility series!

<iframe src="https://giphy.com/embed/3ohryhNgUwwZyxgktq" width="480" height="288" frameBorder="0" class="giphy-embed" allowFullScreen></iframe>

This post has been on the back burner for weeks now, I finally carved out a little chunk of time to write it up. I started a new job at the beginning of February and my personal projects fell by the wayside... But wow I am loving this job! I’m super excited to be working on a team of talented developers, and in a company whose values align with my own.

So here it is, my final Web Accessibility post from the Derek Featherstone series. As I said in my [first post](https://leonorpdx.github.io/web_accessibilty_part_1), this series is based on [Derek Feathstone](https://www.linkedin.com/in/derekfeatherstone/)’s course on LinkedIn Learning, “Accessibility for Web Design.” Check it out, it’s great!

The rest of this post is a synthesis of Derek’s curriculum.

## Flexibility & Accessibility

### Flexible vs. Fixed-width Designs
Over the history of web design, we first designed for 640x480 pixels in the browser display, then 800x600, and then 1024x768. At first we designed like for print, with fixed-width, pixel perfect designs. But even in the early days of the web, this wasn’t necessary: the web is flexible. Rather than confining things to pixel counts, when we could use percentages. Today making more flexible designs is the norm, but there was a lot of debate in the past—especially when designers were coming from a print background. 

It’s important to understand how designing with flexibility in mind is better for accessibility. Fixed width doesn’t respond well to text resizing, which is a common accessibility need. Flexible layouts respond better to different screen sizes; fixed widths designs result in horizontal scrolling. As we’re working on designs, resizing the screen frequently is very helpful for developers and designers; people don’t do this in the real world, of course, but when our design works as we resize then we ensure the web app will work at any window size. This leads us to responsive design.

### Responsive Design & Accessibility
Responsive design is now standard throughout the web development field. It means designing websites and apps that respond differently to being viewed on desktop, tablet and mobile. Most of the time the app is responding to the window size, so the sizes are often renamed to small, medium, and large.

Responsive design is a huge benefit to accessibility: it ensures our apps will work on a wide range of sizes and devices. It is more than flexible vs. fixed width, responsive design also involves considering the flow of content in different devices’ layouts.

Remember: linear content matters, it’s hugely important to accessibility needs, such as using keyboard navigation. The tab order needs to make sense at all sizes, including when content reorders as device sizes change; for example, it’s common for multiple columns to reflow to a single stack on mobile, or for the navigation layout to change. Linear content order is especially important in navigation: on desktop browsers the menu is usually at the top, while on mobile browsers it’s common for the menu to slide in from the left. A user would expect forward-tab to move down the list, but that’s not always the case; careful maintenance of linear content can help prevent confusing and difficult to navigate websites.

Here are some further considerations for responsive design:
* Prioritize content for smallest size: if content is not necessary on smallest size, do you need it on the largest? Probably not, make it cleaner by removing the unneeded content.
* It’s possible to disable pinch-to-zoom on touch interfaces—don’t do this! Some people **need** to be able to zoom in order to use the touch interface. Design your interface to work when zoomed in rather than disabling this feature.
* See this article on [Responsive Content](http://simplyaccessible.com/article/responsive-content/) for more guidelines

Remember that viewport size means just that: the size of the viewport, and nothing else. Consider users with hearing, vision, and/or dexterity issues may use a large monitor with 800x600 resolution, but they are **not** using a tablet. The viewport size cannot be used as a proxy for anything, and developers can’t assume a “tablet” size viewport is touch-responsive. Some people don’t have a monitor at all, so they have no awareness of the size of their viewport—they could be viewing your app on a small/mobile viewport all the time.

When you do know the viewport size, don’t assume small is touch or large is not touch, don’t assume they will use a keyboard... Don’t assume anything beyond the size of the viewport, and make sure it works well in any situation.

### Designing for Text Resize
One of the things we do well in responsive design is accommodating flexible widths. That’s good, it allows a variety of line sizes, text sizes, and zoom levels. One of the things regularly left out in responsive design, however, is accommodating flexible height as well. Some people with low vision use the browser’s text resizing setting, sometimes **in addition** to the magnification setting. Zooming in on entire page content can cause horizontal browsing; zooming in text only changes the text only, and doesn’t change the content flow. 

With fixed height on containers, text will get cut off when a user adjusts zoom on text only. Instead, a **relative** container height will respond to the changing text size and resize accordingly (using rem or em in CSS).

When testing your site, check the following on text resize:
* Content doesn’t disappear
* Content doesn’t overlap
* Content doesn’t lose any meaning or functionality
* Content blocks do not become so narrow as to be unusable

This check should be done not just on components containing plain text, but also forms and all other kinds of content with text content. Test by resizing text up to 200% on text only in the browser settings, not by zooming in on the entire interface.

## Structured Content
### Content Hierarchy & Headings
Users often orient themselves to the page and its content using the page’s format and structure, and scanning for important landmarks like headings. People with disabilities do the same thing: they scan the page to orient themselves and decide whether or not to read the entire page or selected content, and whether they want to read it now or save it for later. The difference is that users with disabilities may use different tools while doing this, like a screen reader or magnification.

Consider this scenario: A screen reader user lands on an article page. She presses 1 to go to the `<h1>` content, likes the title so she tabs to read author and publish date. She’s still interested so goes on to read the intro; partway through uses INS + T to check the page title and the site that she’s on is a reliable news source. She also wants to check how long the article is so she switches to headers and tabs through to read each of the sub-headers in the article. She decides it’s too long to read right now and saves it for later.

This is very similar to how you might access a news article a friend sent to you, just using different tools. People with other differences, like literacy and comprehension difficulties, will rely on heading structure as well. To help all users easily orient themselves on your site, make sure the important content is at the beginning and use the header hierarchy to organize content in a logical manner. Ask yourself: If someone just reads the headings on my page, will they get a good sense of the content of the page? If they read just the first sentence in each subsection, will they have a sense of its content?

A helpful tool is the web developer extension for Chrome which allows you to examine a document outline; there are a number of other similar tools that you can use in other browser environments. Using a tool like this, look at the outline of your page: does the outline tell the “story” of your page? If so, you’re on the right track to make your page usable for everyone. If not, you have some work to do.

### The Importance of Page Titles
Page titles are useful for everyone, they orient the user to what the page is about. They appear in a tab at the top of the browser, and are the first thing a screen reader announces when accessing a page. The titles need to differentiate each page: on a small business site selling various lawn care packages, each package’s “About” page should differentiate itself in the page title. Other pages, like contact and purchase, should also have descriptive titles.
* Packages - Green Thumb Lawncare
* Locations - Green Thumb Lawncare
* Contact Us - Green Thumb Lawncare

Page titles should read specific to general; by putting the distinguishing information first we help people quickly confirm they are on the page they want to be. Page titles should work with the page’s `<h1>` to give the user a clear sense of what the page is about.

### Page Structure
Clean markup on the rest of the page completes the accessibility of your pages’ content and design. Take advantage of HTML5 elements, like `header`, `footer`, `aside`, `article`, `paragraph`, `main`, `nav`, and `button`. Use `table` for tabular data like an order summary, use `ol` and `ul` ordered and unordered lists.

The least meaningful elements are spans, divs, and sections. Use those for elements where there is no other logical element. In all other cases, use the element that makes the most sense. All kinds of assistive technologies rely on working with these elements. Integrating the appropriate elements in your markup also makes your work more efficient, so it’s a win-win.

## Next Steps
Accessibility is hugely important, and all too often it’s left out; just by engaging with this content, you have taken a great first step to start incorporating accessibility into your daily work. Many people say they want their work to prioritize accessibility, but don’t make the changes: review this content every couple weeks, and pick a new habit to integrate into your work. In a year, your products will be vastly more accessible and user-friendly for everyone.

Improving habits, processes, and expectations around accessibility creates an environment that values accessibility and holistically integrates accessible design into every aspect of your products. There are many excellent resources and practices are always evolving; stay up to date by following accessibility leaders on LinkedIn, Twitter, and your regional professional networks.

## Conclusion
And that wraps up my series on web accessibility! I really enjoyed this broad overview of accessibility principles and practices, and I’m excited to see where accessibility improvements will take us in the coming years. Thanks for reading, and happy coding! 
