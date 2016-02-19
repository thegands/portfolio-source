---
layout: page
subheadline: Development
title:  "My First Pull Request"
teaser: "I fixed a navigation bar bug!"
breadcrumb: true
tags:
    - jekyll
    - portfolio
    - feeling-responsive
categories:
    - blog
header: no
image:
    title: first-pull-request.png
    thumb: first-pull-request-thumb.png
comments: true
---
During my coursework on learn.co I have submitted [numerous GitHub pull requests][1] but the vast majority have not been merged[^1] and none of them have been for an actual bug fix. I am still very much learning and trying to get comfortable with all the aspects of web development. A week ago, I wouldn't have believed that I would even be capable of finding a bug in a popular piece of software on GitHub, let alone fixing it. But just a couple days ago [I submitted a pull request][2] for a bug in the [Jekyll theme][3] I am using for my site and, to my delight, the author accepted it!

<blockquote>A week ago, I wouldn't have believed that I would even be capable of finding a bug in a popular piece of software on GitHub, let alone fixing it.</blockquote>

The bug itself was a disappearing element in the navbar that would only occur on medium size screens. I first noticed it when I was navigating my website with Chrome's developer tools open on the side. I immediately felt anxiety because my first thought was that I screwed something up and I would have to figure out and undo the foolish mistake I had made. Then it occurred to me that it could also be affecting the [»Feeling Responsive«][3] theme I am using. I was relieved to see the bug present in the [»Feeling Responsive«][3] demo page and I realized I had an opportunity to help improve a piece of software that helped improve my portfolio tremendously! But first I had to figure out what was causing the bug and how exactly to fix it.

## Developer Tools to the Rescue!
I wasted a lot of time trying to figure it out but eventually I found that if I inspected the element that was disappearing I could view it's class names. One stood out to me as a good descriptor of the element that was disappearing: `parent-link`. Once I was armed with a good descriptor keyword I searched for it in all of the source files of the author's branch of my code's repository[^2]. I suspected it was a JavaScript function that was causing it to disappear and sure enough the only result was a JavaScript file! I saw that the JavaScript function was inserting the mobile subnav elements with the foundation class `show-for-small-only`. That class appeared to be causing that particular element to only show on small screen devices when I needed the element to show for small screen devices *and* medium screen devices.

## When in doubt, read documentation!
After a quick look in the [Foundation Documentation][4] I found the class `hide-for-large-up` and tried it out. I was ecstatic when the navbar was behaving correctly and I promptly proceeded to submit my pull request. I was hasty and bungled up the fix in my first pull request so I closed it minutes later.<sup><sup>LoL</sup></sup> I had it figured out eventually though and a few hours later I got a reply from the [author himself][5]! All I had given him to describe the bug was the one line message I had included with my git commit so I wasn't surprised when he was curious how he could trigger the bug and see it for himself.

I was too lazy to type an explanation and since I'm familiar with [streaming video games on Twitch][6] I already have software setup to capture my computer screen so I whipped up a <a href="#" data-reveal-id="videoModal">quick video and threw it on YouTube</a> to be assured I would get the message across. I submitted a reply in the comments section of my pull request and went to bed. Even though it turned out to be a simple bug I had really lost track of time going down the bug fix rabbit hole, I had nearly stayed up all night! The first thing I did when I woke up was check on my pull request and saw that the author accepted and merged it. His demo page for his theme was even working correctly! The collaborative nature of GitHub is amazing and addicting and now I feel like I am officially a part of it <i class="fa fa-smile-o"></i>

## Setting Goals
I realized that by figuring out a fix for the bug and submitting the pull request, I was, in a certain regard, doing the author's work *for* him! Due to the public nature of GitHub I am credited for the work but more importantly I get gratification from helping to improve a piece of software that has helped me. This has given me inspiration to eventually create a piece of software that is useful enough to be noticed and receive a pull request. It would be amazing to see that my work would be appreciated enough by someone else that they would improve it *for* me. I will have to figure out a good idea but the more I learn the more confident I will become when trying to implement an idea so I'm confident I will achieve my new goal!

## Other Posts About Feeling Responsive
{: .t60 }
{% include list-posts tag='feeling-responsive' %}

<div id="videoModal" class="reveal-modal large" data-reveal="">
  <div class="flex-video widescreen vimeo" style="display: block;">
    <iframe width="1280" height="720" src="https://www.youtube.com/embed/Z55XIV14doE" frameborder="0" allowfullscreen></iframe>
  </div>
  <a class="close-reveal-modal">&#215;</a>
</div>

[1]: https://github.com/search?q=author%3Athegands+is%3Apr&type=Issues
[2]: https://github.com/Phlow/feeling-responsive/pull/87
[3]: http://phlow.github.io/feeling-responsive/
[4]: http://foundation.zurb.com/sites/docs/v/5.5.3/components/visibility.html
[5]: https://github.com/Phlow
[6]: http://www.twitch.tv/thegands
[7]: https://github.com/Phlow/feeling-responsive/

[^1]: GitHub pull requests are used by learn.co to submit and record students' work.
[^2]: I forked [»Feeling Responsive«][7] to create my website so the all I have to do is change branches to view the author's code!
