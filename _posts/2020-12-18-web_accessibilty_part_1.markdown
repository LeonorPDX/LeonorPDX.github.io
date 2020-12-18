---
layout: post
title:      "Web Accessibilty: Part 1"
date:       2020-12-18 16:13:17 -0500
permalink:  web_accessibilty_part_1
---


For my next few blog posts, I’m going to write a short series on accessibility in web development. Accessibility is critically important and one of my personal values that I seek to integrate throughout my professional practice.

These posts are based on a video series I recently watched through LinkedIn Learning, called “Accessibility for Web Design.” It’s a great learning resource, developed and presented by [Derek Featherstone, Accessibility and Inclusive Design Leader](https://www.linkedin.com/in/derekfeatherstone/). I highly recommend you check out the course if you can, or look at other resources Derek provides. Derek has a wealth of knowledge in this field and presents the content in an engaging and effective manner.

These blog posts will summarize Derek’s content at a high level and are not intended to replace his course; I’m writing these posts as a way to cement my learning of the content and as a reference source that I can return to as I implement these accessibility practices in my future work.

I also hope that some of my readers will find these posts to be a useful introduction to web accessibility, and that it will spark an interest that leads them to continue learning about this topic and integrate accessibility into all aspects of their work.

The rest of the content in this post is a synthesis of Derek’s curriculum.

## Intro to Accessibility and the Web

In order for digital experiences to be fully accessible, accessibility best practices must be applied throughout the design and development process. There are accessibility best practices that can be applied to web development (the actual code being written in an accessible manner), visual design, and content development. Together, each of these disciplines play a role in the overall accessibility of a web application.

In addition to thinking about accessibility in multiple contexts, we also should be thinking about accessibility everyday. When web accessibility is integrated into an application’s design from its inception, the app’s accessibility will:
* be more complete and successful,
* take less time and expense than retrofitting accessibility fixes on a finished application, and
* create a more seamless, integrated experience in which people with different accessibility needs don’t lose out on any of the app’s experiences.

This blog post will cover some of the basics of accessibility as it relates to visual design.

## Note on Personas

It is useful to create personas to help web professionals conceptualize the users’ experiences and needs; personas are imagined users and include the user’s mindset, motivation, and functional needs.

In order to give us an understanding of mindset and motivation, personas typically include information about the user’s age, gender, background, values, and behavior patterns (e.g., an early-adopter of new tech, a busy career-person who does all their shopping online, etc.).

Functional needs include diverse abilities, such as users with no vision who rely on screen readers, users with low vision who view the screen at 400% magnification, users with severe arthritis who rely on keyboard navigation, users with Autism Spectrum Disorder, and any number of other cognitive or physical differences.

Derek gives 5 great examples of rich, interesting personas that paint a picture in your mind and make imagining these users’ experiences easier.

## Visual Design Accessibility

### Color

Color blindness impacts a large portion of the population: 1 in 12 men and 1 in 200 women experience some form of colorblindness. Color can also impact accessibility for low or no vision users. For further resources on designing for accessibility around color blindness, Derek recommends Geri Coady’s book “[A Pocket Guide to Colour Accessibility](https://www.fivesimplesteps.com/products/colour-accessibility).”

Since not all people see colors in the same way, it is important that color is not the sole way of conveying information. Examples include:

* Color swatches of products
* Color to indicate different datasets in graphs
* Color to indicate categories of posts or products
* Red or green to indicate failure or success

It isn’t that we should not use color—color is great, and conveys a lot of information—we just need to include *other* ways of conveying the same information as the color. For example:

* Shapes and symbols
* Line patterns in graphs
* Labels 

Color contrast can also have a big impact on accessibility, and usability for all users. Color contrast refers to the colors in the background and foreground (such as page and text color) and the relative brightness and luminosity of the contrasting colors. Minimum accessible contrast ratios are outlined in the [Web Content Accessibility Guidelines (WCAG)](https://www.w3.org/WAI/standards-guidelines/wcag/). There are many [free contrast checkers](https://contrastchecker.com/) that can check your color palette against WCAG and other accessibility guidelines. When making style guides, check the contrast of all colors in the palette; be sure to indicate which colors that can be used in combination for foreground and background, and which colors should not be used together.

### Animations & Flashing

Animations have been known to trigger vestibular disorders symptoms. Have you ever stood at the foot of a very tall building, looked straight up, and felt disoriented? Experienced dizziness, vertigo, maybe even headache or nausea as you lose all orientation to the ground? Animations, such as video backgrounds, slideshow content, and so on can trigger similar feelings in people with vestibular disorders.

Designing to accommodate vestibular disorders is one of the newest areas of advancement in accessibility, and we will likely see improved recommendations on design best practices as we learn more about how design relates to vestibular disorders. Here are some solutions that developers and designers are already employing:

* Include an option to turn off animation or videos. Some operating systems and browsers have an option to automatically turn off animations; you can code your app to check for this option and change its interface accordingly.
* Consider dissolve/fade transitions instead of motion
* Make sure the content and functionality of the app do not rely on animation to convey meaning
* Design stateful animation that has meaningful start and end states

Most people already know that flashing can trigger seizures; we often see warnings at the start of some movies or amusement park rides. Obviously we should not deliberately include flashing in videos or animations as part of our web design. However, it is important to note that some scripts or state changes that trigger a repainting of the DOM can also cause flashing.

*Note from Leonor: You can use free, automated accessibility scanners to check for flashing; it’s also a good idea to use a “loading” component/class/state to control the screen and help prevent flashing.*

### Grouping

Grouping is the idea that related content should be proximal. Consider users using 200%, 400%, or even higher magnification—they only see a fraction of the window at any given time when navigating your app with magnification. Some examples where this might be challenging:

* Using a slider to move a video to a specific time point, but the time indicator on the video is only visible at the beginning—when zoomed in on the slider, the user can’t see what time they are at in the video. **Easy solution:** have the time indicator appear as a tab directly above or below the slider.

* "Add to cart" button causes the cart icon in the upper right corner to change color briefly and increments the number showing how many items are in the cart; however, nothing happens visually right around the “add to cart” button, making it difficult to tell if you have successfully added the item to your cart when viewing the screen with magnification. **Easy solution:** have the “add to cart” button indicate success in addition to the cart in the upper right corner, using something like a check mark, success color, a short success message, or combination of these methods.

A quick and easy way to check for grouping accessibility is the “straw test.” Users with low vision often say it’s like looking at the world through a straw; they have a very narrow field of vision. To simulate this, close one eye, form a loose fist, and hold your fist to your open eye so you can look through the opening formed between your palm and curled fingers. By doing so, you can narrow your field of vision and simulate a low vision user’s experience who relies on high magnification.

The straw test can be used at all levels of design (wireframing, development, etc.) and for walking through or testing out all functionality of your app.

### Visual Focus Indicators

Visual focus indicators are used when navigating through a web page with the keyboard. Some keyboard navigators also use screen readers, like someone who is completely blind—they need good keyboard navigation functionality and labels that their screen reader can use. Others use the keyboard navigation but don’t have any vision impairment—aria labels and screen reader accessibility won’t help them, they need to be able to *see on the screen* where they are currently focused.

Focus state is a visual indicator to help these users; it is usually an outline. Web browsers have built in focus indicators, however, those indicators are not always visible in the design of your site; a grey outline won’t be visible if you already have a dark background.

It’s best to write custom focus states in your CSS that are highly visible on your app’s design. Derek recommends that you use `outline: none` to override browsers built-in focus state and set your own focus state using `:focus`. For example, in my portfolio site I use the following focus state in my CSS file:

```
:focus {
    outline: 3px solid var(--clr-accent);
    outline-offset: 3px;
}
```

## Conclusion

That’s all for visual design accessibility! Next week I’ll provide an overview of keyboard functionality and touch interfaces. Happy coding!

