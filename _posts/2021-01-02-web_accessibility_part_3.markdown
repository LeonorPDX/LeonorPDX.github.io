---
layout: post
title:      "Web Accessibility: Part 3"
date:       2021-01-02 17:07:47 -0500
permalink:  web_accessibility_part_3
---


We made it to 2021! 

But on a serious note for a moment... This past year has been hard in a lot of ways: political and social upheaval, devastating wildfires and climate-related disasters, racial violence and the ongoing fight for racial justice, to name just a few. My hope is that we take these pangs as a call to action, and that we don't "go back to normal." I'm reminded of Steven Pinker's ***The Better Angels of Our Nature: Why Violence Has Declined***  and especially his argument that the continued decline of violence is not inevitable but relies on our empathy and reason. I hope recent events will catalyze our empathy and hone our reasoning, and that we take the necessary steps to build a better world for all of our community, our children, and children's children.

To that end, let's make a small difference in our corner of the world by building a more accessible digital landscape.

This is the 3rd post in my series on web accessibility. As I said in my [first post](https://leonorpdx.github.io/web_accessibilty_part_1), this series is based on [Derek Feathstone’s](https://www.linkedin.com/in/derekfeatherstone/) course on LinkedIn Learning, “Accessibility for Web Design.” I highly recommend you check it out!

The rest of this post is a synthesis of Derek’s curriculum.

## Multimedia: Images, Audio and Video

The web used to be a text based medium, but that changed when we gave browsers the ability to display images. Now there are literally hundreds of billions of images on the web and billions of hours of audio and video recordings. On YouTube alone, 500 hours of video are uploaded *every minute*.

Audio and video are very rich media; they convey a lot of very nuanced information. How do we ensure that users with differing abilities get to experience this richness? The first questions we should ask are “what is the purpose of this content?” and “how will people use this content?” 

For example, consider podcasts. Users will want to listen to podcasts; they may also want to download the podcasts so they can listen to them while offline. In another example, consider instructional videos, like a video lesson on how to play the guitar. Users will want to watch instructional videos; they may also need to listen closely to instructions in the audio, be able to see details in the video, and have the ability to pause/rewind easily so they can practice the skills being taught in the video.

Answering the questions about how and why users use multimedia resources will make it easier to make these resources more accessible, and determine what kind of accessibility tools you should employ.

### Content Types

Multimedia content generally falls into one or more of three categories: informational, functional, or decorative.

Informational content conveys important information, maybe about services offered or how to use a product, or the content is valuable in its own right (like a podcast). Informational content needs to be repeated as text. Some podcasts have a summary paragraph or “highlights” list in addition to the audio. These give a high-level overview of the topics covered, but they are not a text equivalent: if a user cannot listen to the podcast, does reading the summary paragraph or list give them the same richness of experience as listening to the podcast? Transcripts are more than a nice bonus feature, they are the only way some users will be able to access the content. Having the transcript in an easily read format that can be downloaded is the best.

Functional content are images or other media that performs some kind of function, like the SVG icons to play or pause an audio or visual recording. All functional multimedia content also needs to have a text equivalent; alt text or aria labels work well for functional multimedia content.

Decorative content does not convey important information or serve a functional purpose. Consider this example from Etsy:

![Capture of Etsy product hierarchy](https://i.imgur.com/IOpvwPL.png?1)

The angle-bracket icons indicate the hierarchy of products from Clothing, to Women’s Clothing, to Tees & Tops. This icon is decorative; the list itself conveys the hierarchy and the icon plays no further informational or functional role. Decorative content does not need a text equivalent or other special accessibility considerations.

It can be helpful to ask yourself, if this media was gone, would the page work any differently or any meaning be affected? If the answer is yes, you need to create alternatives.

### Text Equivalents

Text equivalents are things like transcripts for audio or video recordings, labels for product color swatches, or aria labels and alt text descriptions. As we’ve already seen, text equivalents are a critical part of your app’s overall accessibility. Text equivalents must communicate the same message as their associated visual or audio elements.

Text equivalents are not only used by users with low or no vision who rely on screen readers; voice recognition software used by people with low mobility also use the text equivalents (e.g., a user says “click Women’s Clothing” and the voice recognition software focuses on the element labeled Women’s Clothing and clicks). People that use voice recognition software can use tools like [mouse grid](https://www.nuance.com/products/help/dragon/dragon-for-mac/enx/Content/Navigation/MouseGrid.html#:~:text=The%20MouseGrid%20allows%20you%20to,pointer%20to%20a%20specific%20area.&text=A%20new%20three%2Dby%2Dthree,will%20see%20a%20magnification%20window.) as an alternative to click on something that is not labeled correctly, but it can be very difficult and time consuming—definitely not a good user experience.

You can test your site’s text equivalence by using a screen reader and tabbing through the navigation; does every place a user might click have a meaningful label for the screen reader? Watch out for copy-paste errors like the same label being used on multiple elements in a row. You can also try a voice recognition tool like Dragon to see if you are able to navigate your site by voice.

### Note on Complex Visuals

Some media won’t fall neatly into one of the above categories of information, functional or decorative; it may be a mix of two, or all three. With complex visuals, ask the same questions as above. Consider, what is the overall purpose of this content? What is the best alternative way to convey that? Which parts of this content are informational, functional, or decorative?

Remember that informational content needs a full text equivalent, functional needs a text equivalent that makes its function apparent, and decorative needs no text equivalent.

Working with multimedia can quickly become very complex, but your role as a developer or designer is the same regardless: determine the purpose of the content and make equivalents so the content is accessible to everyone.

## Forms

### Labels

Labeling form fields is the simplest thing to do to make form more accessible and easy to use for **everyone.** Labels:
* Create a programmatic relationship for assistive technology,
* Make clearer prompts for people with low vision,
* Help with mobility and dexterity because they make bigger targets (e.g., when a radio button is labeled you can click anywhere on the label text to select that option, instead of only in the small radio button target), and
* Serve as a memory aid for those with memory related issues.

Every form field needs a `<label>` tag. When a user navigates a form with a screen reader, the screen reader tells the user the name of the form field and what type of control field it is. Labeling the form fields lets the user know exactly what they’re filling out, and the tools they’ll need to complete the field; the user knows they can type in a text box, use the space bar for a check box, and use arrow keys to navigate a list of radio buttons.

For a quick and easy test, view your form in the browser and click every label, checking that the focus moves to the correct field or toggles selections correctly. When a field is labeled, clicking anywhere in the text of the label will bring the focus into the label’s field or select the radio or checkbox.


### The Placeholder Attribute

The placeholder attribute is fairly new, and it is meant to provide a hint on how to complete the field, like the format for date. Placeholders disappear as soon as you click the field, which makes it less useful—especially for people with memory or literacy issues, or for whom the language of your app is not their first language. We can make it more useful by making the placehold persist: simply take the value of the placeholder and append it to the label text when the field is in focus.

There has been a design trend of using the placeholder as a replacement for the label. When placeholders are used with no on-screen labels, the form fields often go unlabeled all together. Don’t do this. Labels and placeholders do two separate things.

Designers started using placeholders in this manner to save vertical space, and to make the form look cleaner with less visual noise. It causes unintended consequences for people with low-vision, cognitive or memory issues; sometimes users think the form is already filled out. Also, the placeholder is lighter, so for some users with low-vision it can appear that the form is completely blank and unlabeled, and they have no idea how to complete the form.

There are some emerging trends to take advantage of the cleaner look and reduced vertical space of using placeholders while remaining more accessible. This solution uses the placeholders and no visible labels, but when a field is in focus the label becomes visible (above, below, or to the side), often as an animated tab that slides out. One caveat, however: there’s not sufficient research yet on how accessible this design is for neurodivergent users, like users on the Autism spectrum. For some users, this design may be completely confusing and unpredictable, and cause a lot of anxiety as a result.

### HTML5 Field Types

With HTML5 we got some newly deployed specialized form fields like tel, email, date, and so on. The special fields are recognized on mobile devices as being different, so the user gets a customized keyboard display that is optimized for entering that datatype (like a numeric pad for phone numbers, or a keyboard with the @ symbol and ‘.com’ for emails). This feature makes forms much easier to fill out for everyone, including people with differing abilities.

To take advantage of this feature, make sure that specific field types are included in the initial design phase and used in all phases of development. When you consider specific field types from the earliest stages of design and development, it is less likely for this important detail to get missed.

### Validations and Error Messages

We’ve seen that form fields need labels and correct field type. They also need to have some way of validating that the input the user has given fits the input requirements, and shows a meaningful, accessible message to the user if there’s something wrong with their input. It’s not as simple as just making a message appear somewhere on the page; errors should be consistent, discoverable, understandable, and programmatically connected to the field with the error.

Error messages should be placed in a consistent position and always use the same error icon. Displaying errors like this is better for everyone and it’s especially helpful for people that rely on consistency and predictability. Using a consistent icon is also useful for screen readers that will announce the icon by its aria label “error.”

The error needs to be close to the field in error so it’s easily discoverable. Remember the straw test from the first post in the series, and how viewing the screen at magnification can affect how a user is able to perceive and understand content.

When displaying the error messages, make sure they use plain, simple language; this is easier for everyone to understand, and it’s especially helpful for users with cognitive or linguistic differences.

The error should be programmatically connected to the field with the error so assistive technology can focus on the correct field. A common solution is to have the error appear in the label of the form field.

And lastly, consider how your errors handling will behave in different scenarios (very long errors, when the page is viewed with different text size or magnification, language translators, etc.). It’s a lot to consider, but if you *do* consider it, you’re doing your job really well.

## Conclusion

That’s all for this week, thanks for reading! Next week we’ll wrap up the series with a post on flexibility and how responsive design relates to accessibility, and content structure.
