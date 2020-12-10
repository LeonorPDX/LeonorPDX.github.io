---
layout: post
title:      "Portfolio Site is Live!"
date:       2020-12-10 15:32:40 -0500
permalink:  portfolio_site_is_live
---


I recently built a portfolio site to showcase some of my personal projects, you can check it out [here](https://www.leonorcolbert.com). I built the site from scratch using HTML, CSS, and some simple JavaScript.

<iframe src="https://giphy.com/embed/ytbg9ZKXhD6lw7ebRV" width="480" height="260" frameBorder="0" allowFullScreen></iframe>

When it came to creating my portfolio site, I read lots of blogs and got tons of advice from colleagues and mentors. This blog post is a synthesis of all these difference sources as well as my thoughts on important things to do when building your first portfolio site.

## Keep it simple and user friendly

First, keep it simple and always keep the user experience in mind. A single page should have everything a visitor needs to see (who you are, what you do, how to reach you).

For your text content, make sure you keep it brief: in general, people will only read short blocks of text. When users see a longer block of text they tend to skip it entirely rather than reading any of it. **By saying less, you will communicate more.**


## Include your portfolio projects (obviously)

There are some different opinions on how many projects and what kind of projects you should include. In my opinion, I think it's best to include any finished project or partial project that showcases your skills; don't include anything you copied or any projects that don't work or don't follow programming conventions and best practices. Also remember that visitors likely won't spend nearly as much time looking at your portfolio as you'd like to believe—put your best projects at the top of the list.

If your projects are deployed, you can provide external links; you should still include a brief description and link to the GitHub repo on your portfolio site so visitors can get a feel for the project without leaving the current page.

If you have not deployed your projects, you can have individual show-pages for each project that includes video clips of the project in action and more detailed information. You can write a little more in these sections and go into detail  about the technologies used, successes and challenges.

In my portfolio site, I opted to provide a brief top-level description on the project show page and linked to my blog post about each project. The blog posts give a detailed description of my process, the technologies used, and challenges I overcame.


## Be “on-brand”

First, you have to figure out what your brand is. Your photos, “About Me”, colors, and overall design should all work together to communicate your brand identity.

A quick web search will bring up lots of tools to help you develop your brand identity. [This article](https://fabrikbrands.com/how-to-create-a-brand-identity/) is a good overview of questions to consider when developing your personal brand; while written for companies, it can easily be translated to individual professionals’ branding.

Once you've done some work on identifying your personal brand, try to distill its essence to a few key-words or a key-phrase. For instance, I consider my brand key-words to be: big-picture thinker, detail oriented, and interpretation specialist. My key-phrase could be that I “never lose sight of the forest for the trees,” meaning I excel in big-picture thinking while maintaining close attention to detail; this phrase also captures my affinity for the outdoors and love of language.

Choosing color and design elements that align with your brand doesn't have to be hard; it can happen organically as you simply select colors and designs that appeal to you. You can also do web searches for the key words of your brand plus "color scheme" or "design scheme." There are lots of great resources to help you generate ideas like this [blog post](https://visme.co/blog/website-color-schemes/) on color schemes.

And lastly, be sure to write in a voice that embodies your brand—is your brand Creative? Corporate? Jack/Jill-of-all-trades looking for scrappy start-up opportunities? If you need ideas on tone, look at company websites that reflect your brand and pay attention to their communication styles.


## Use a custom URL

There are lots of reasonably priced options for domain registration and web hosting. I used [Netlify](https://www.netlify.com/); they provide domain registration and hosting all for one low annual fee. Plus you just link Netlify to your GitHub repo and they will automaticly deploy updates triggered by changes to the GitHub repo. Plus Netlify automatically issues a SSL certificate, making your site more secure.

Can also use other hosts like Digita Ocean or Heroku, or purchase your domain registration from a domain registrar like GoDaddy and then point the DNS to a GitHub page. Personally, I found Netlify to be a really quick, painless way of getting a site up in a matter of minutes. 

## And finally: design. To template or not to template?

If you need a site up *fast*, like for some reason you need your portfolio up in less than an hour, use a template. My one word of cautionary advice is to pay close attention to details and fully customize it to your site; for instance, I often see portfolios built from HTML templates where the developer never updated the page title in the HTML head, so it still reads a generic name like “Professional Portfolio, Dark Theme.” Not great.

In most cases, it is best to build your site from scratch. Afterall, you are putting this portfolio out there to demonstrate that you are a web developer, right? And it doesn’t have to take forever or be a headache; with so many resources and tutorials, you can get a nice looking site coded in a matter of hours. There are plenty of tutorials you can use as jumping off points, just try to customize your site in some way. For instance, I used a tutorial from Scrimba for my basic design, but I changed the behavior of the menu to respond the the window size and changed the format of the work section to include info as well as images, and changed the images from flex to grid display in the CSS so they don't get distorted on iPhone users' mobile browsers.

You can also opt to use a WordPress template and customize the theme. As long as you make the effort to customize the theme and template, this too can be a great way to showcase your skills. Many clients will use WordPress or other CMS platforms rather than asking for a developer to build a site/app from scratch, so showcasing your ability to create customized sites on a CMS platform is valuable.

Whether you use a template, code from scratch, or customize a CMS template, always design mobile-first. Most users will view your site for the virst time on a mobile device. As you are designing the site, always be playing with your browser window size. You can also use cross browser compatability testers, like [Lambda Test](https://www.lambdatest.com/), to see how your site would behave on different devices, operating systems, and browsers.


## Bonus: Accessibility and usability!
Both accessibility and usability refer to how users understand and navigate your site.

Accessibility refers to users’ ability to understand and navigate your site when they have some differences in their abilities such as low or no vision, limited mobility, etc. Some people with different abilities may use assistive technology, like screen readers. There are many free accessibility scanners that will check your site against Web Content Accessibility Guideline (WCAG). Accessibility scanners check that your site meets minimum requirements such as font size and color contrast, alt text on images, descriptive links and buttons, and key-board navigation. Most of the free scanners can't provide a definitive analysis because they are automated; accessibility testing is most accurate when a human user can experience the site and employ common sense. However, free scanners will give you a starting point though and flag obvious issues.

Usability refers to the user experience for people of all ability levels. You designed your site with the user experience in mind, but sometimes things fall through the cracks! I found that the best way to test usability was to “soft launch” my portfolio and share it with a few select networks via Slack or LinkedIn groups, such as alumni associations and professional affinity groups (“Women in Tech” etc). People are so generous with their time and expertise, and I received a lot of invaluable, detailed feedback from these groups within 1 day of sharing my portfolio to the group asking for feedback.

Most junior web developers only have limited knowledge of accessibility best practices, so doing a little extra research in this area may help set you apart from the crowd when it comes to job applications and interviews.


## Conclusion

The most important thing is to just to get a portfolio out there. It is an essential way to show what you can do as a career-changer, self-taught developer, or grad of a bootcamp. Have fun doing it, and be proud of your work!

