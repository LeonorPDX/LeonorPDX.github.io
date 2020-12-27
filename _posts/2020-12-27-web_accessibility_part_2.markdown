---
layout: post
title:      "Web Accessibility: Part 2"
date:       2020-12-27 19:57:08 +0000
permalink:  web_accessibility_part_2
---


Happy holidays and best wishes for the new year! 2020 has been a doozy of a year for lots of reasons; despite being VERY ready to say “good riddance to bad rubbish!” to 2020, I recently sat down and journaled about some of the things I’m grateful for this year. I found that despite my often feeling anxious or frustrated, I actually have a lot to be happy about from the last 12 months—not the least of which is starting my new career as a web developer! I hope you also have some positive things to take away from the past year, and have new things on the horizon that have you excited as we head into the new year.

This is the 2nd post in my series on web accessibility where we’ll focus on keyboard navigation and touch interfaces. As I said in my [first post](https://leonorpdx.github.io/web_accessibilty_part_1), this series is based on [Derek Feathstone’s](https://www.linkedin.com/in/derekfeatherstone/) course on LinkedIn Learning, “Accessibility for Web Design.” I highly recommend you check it out!

The rest of this post is a synthesis of Derek’s curriculum.

## Keyboard Navigation

Users might rely on keyboard navigation for a range of reasons; some use assistive technology like screen readers and switches, others can see the screen but use the keyboard to navigate the web. Many different disabilities related to mobility, dexterity, or vision result in the need to use keyboard navigation. When designing accessible digital experiences, users must be able to complete all tasks and access all content via keyboard navigation (i.e., using the tab key and shift-tab to move forward and backward between focus points).

Sometimes developers interpret the need for keyboard navigation as meaning that each paragraph or image needs to have a keyboard focus that the user can tab to, but that’s not the case. Keyboard navigation does *not* mean every bit of content needs to have a keyboard focus—just that any functionality like completing a purchase, searching content, or opening an expanded content feature like an accordion must be accessible via keyboard.

To test your app for keyboard navigation accessibility, view your app in the browser and use the tab key to navigate the page. Can you tab to everything that can be clicked? It’s especially interesting to test keyboard navigation on embedded frames, like videos or maps.

As you test the keyboard navigation, you’ll find that some HTML tags have a keyboard focus you can tab to, and others do not. For instance, you cannot tab to a `<div>`. Sometimes a `<div>` is styled as a button—don’t do this! Links, buttons, and forms all have built in accessibility. There is rarely, if ever, a need to attach functionality (via a JavaScript function) to `<div>`.

### Keyboard Traps

A keyboard trap is something a user can navigate into via the keyboard, but cannot get out of in some or all cases. The user needs to close their browser and start over. Audio and video players, sliders, accordions, and maps can all create keyboard traps and should be checked thoroughly. Here are a couple examples of potential keyboard traps:

* On an ecommerce site’s payment page, there’s a field to enter the security code from the user’s payment card. This field has JavaScript that requires users to complete the field with correctly formatted input. A user that relies on keyboard navigation is completing the form and sees that they mistyped their card number after advancing into the security code field and tries to use shift-tab to go back and correct the card number. The field won’t let them tab forward or back without inputting the 3-digit security code, but the user just experiences being stuck with no indication why they cannot move out of this field.

* A contact form has an input field for phone numbers with a field for each block of digits: [XXX] - [XXX] - [XXXX]. The form uses JavaScript to auto-advance the input focus to the next field after a user inputs 3 digits. The user cannot use shift-tab to go back into the previous field because they are instantly auto-advanced to the next field again by the JavaScript. Blind users often rely on their screen reader to read back what they have just entered to confirm that everything was entered correctly; a user won’t be able to do that with the JavaScript written the way it is.

These examples are a result of poorly written JavaScript. It’s not that we shouldn’t have these features, they are helpful to a lot of users! But they should be written with keyboard traps in mind and tested to make sure they don’t cause accessibility issues. Whenever possible the best testing is by users with disabilities, they will do a better job catching the problems than people trying to put themselves in the place of a person with disabilities.

To make keyboard navigation easier to implement, write out the tab order for keyboard navigators during the wireframing stage; this way developers will have a map of how navigation should work. Writing out a keyboard navigation flow will make the developers’ jobs easier and forgotten keyboard traps will be less likely. Make it a baseline expectation for everyone on the web development team to always be thinking of keyboard navigation.

### Linear Content

Related to the tab order in keyboard navigation, content should be organized in a logical, linear flow. Screen readers will read the page content back to the user in the order it is written in the source code. Additionally, some neurodivergent users expect page content to follow a logical and predictable pattern from top to bottom, and left to right.

Consider an example on an ecommerce site’s page for placing orders. The content is displayed in two columns; on one side is the order summary and shipping address associated with the order, and the other side is the fields to input billing info and submitting the order. Which should the screen reader read first?

In this example, it makes sense for the screen reader and tab navigation to reach the order summary first, then input billing info. It may seem obvious, but if designers and developers don’t consider keyboard navigation and screen readers in all their choices, this page could easily end up with the content in the reverse order which would not be easy for keyboard users. Fixing problems in the linear content flow usually is very straightforward and doesn’t need to create extra work in updating CSS or other related files. Fixing content flow usually only requires cutting the code that is out of sequence and inserting it into the appropriate place for logical content flow in whatever file generates the HTML that renders on the DOM.

## Touch Interfaces

Touch interfaces are ubiquitous, and will only become more common in coming years. The frequency of implementation, abilities, and sizes of touch interfaces have all rapidly increased over recent years. Touch interfaces with built-in accessibility features has also been on the rise, and we will likely see more innovation in this area in the future.

When considering touch interfaces, it’s important to remember that *we’re not all the same.* Consider how you use your touch screen—do you hold it with one hand, or two? Swipe with a finger or thumb? Type with one hand or two? Does it change? Now consider a user who uses their feet to manipulate their touch screen, or uses their nose, or uses voice recognition software and a bluetooth keyboard and doesn’t touch the screen at all—how does this change what the “perfect” touch interface might look like?

Bottom line, always test your apps with diverse users and consider that some users never touch the screen at all. 

### Target Sizes

Target sizes can have a big impact on accessibility in touch interfaces, especially touch targets that are smaller such as results pagination, radio buttons, or dropdown menu selections. To make these smaller targets more accessible, make them bigger and give them more padding. This makes them easier for everyone, regardless of abilities.

Microsoft, Apple and Google all have guidelines on minimum target size in touch interfaces ranging from 34x34 to 48x48 pixels. There’s some debate if these are still too small, so when it doubt just make the targets a little bigger and farther apart,

### Gestures

Gestures are common in touch interfaces, things like pinch or spread two fingers to zoom in and out, slide to confirm, or swipe table to sort columns’ data to be ascending or descending.

Not every user will be able to do every gesture, so it’s important to build in alternatives like double tap to toggle column order to to move a confirmation slider along its track. With every gesture-based function, ask: How would this work if the user *cannot* do the gesture?

Developers can also do things like make the code that checks the slide-to-confirm gesture more generous, such as still accepting the gesture if the user moves outside of the slide field or overshoots the end point. This will make it easier for users with dexterity issues to use the gesture.

Microsoft, Apple and Google all have documentation on their guidelines for accessible touch interfaces. These guidelines are good resources to look at when working on your own accessibility as it relates to touch interfaces.

## Conclusion

That’s all for this week, thanks for reading and taking interest in accessibility on the web! Next time we’ll go over images and multimedia content.

