---
layout: page
subheadline: Development
title:  "Starting my Portfolio!"
teaser: "All Jekyll and no Hyde"
breadcrumb: true
tags:
    - jekyll
    - portfolio
categories:
    - blog
header:
    image: header_unsplash_2-970x.jpg
    pattern: pattern_worn_dots.png
        #thumb: gallery-example-3-thumb.jpg
        #title: gallery-example-3.jpg
        #caption_url: http://unsplash.com
---
After many more hours than I care to admit my portfolio and blog is finally live! I created this site as an easy way to record and publish my thoughts on web development as I am learning with additional pages dedicated to showcasing my personality and coding abilities. So for my very first blog post I decided to write my thought process on the creation of this site!

I started by trying to assess what I wanted and what tools would best assist my development. Since I want to present a website that will get me hired as a web developer I thought it best to think as if I were a potential employer looking at portfolios in order to fill a junior web dev position. I would expect employers are looking for:

* Correct use of HTML5 and CSS3

* Responsive design

* Aesthetic pages with clear navigation

* Live code examples with links to repositories

## WordPress, because everyone's doing it

The first tool that came to my mind was [WordPress][1]. WordPress can easily meet the above requirements and is incredibly simple to deploy and add content to, complete with an admin dashboard to manage and add content all within the comfort of your web browser. The biggest drawback of WordPress (and most almost all CMS systems) is it requires a server that runs server side scripting (PHP in the case of WordPress), which nearly always costs money.

Even though I recently bought a year of shared hosting with PHP support I did not choose to use WordPress. I did this for several reasons:

* I want as much uptime on my portfolio as possible. I would not expect an employer to attempt to revisit a portfolio if it does not load the first time they try to access it. Websites that rely on PHP are vulnerable to an additional point of failure as opposed to a static website.

    - If I am unable to pay for web hosting my portfolio will be taken offline which could hurt my available opportunities.

    - The WordPress naysayers will always mention that WordPress is notorious for being hacked and requires diligence in keeping up to date. A hacked WordPress page in my name is not something I would want a potential employer to see under any circumstance!

* For use on a web development portfolio, WordPress might as well be considered cheating. It does not require knowledge of HTML, CSS or PHP. Most paid hosting providers even have one click installers for deploying a WordPress site in seconds.

* Deep customization of WordPress can be a pain with a plethora of plugins available to attempt to address the most popular missing features. However, plugins can add unneeded complexity and open up additional security vulnerabilities.

## Enter Jekyll

Jekyll really felt like a no brainer. [Jekyll][2] is a static site generator designed for blogs. I can have a collection of source files that represent my website with all my blog posts and dynamically compile them into a working static copy of my website. This makes it perfect for hosting on GitHub Pages, [GitHub even has official support for Jekyll][3]! Hosting static pages on GitHub ensures nearly 100% uptime and is also completely free! Jekyll was developed in Ruby and I am gradually becoming familiar with exactly how amazing and powerful Ruby can be as I learn about it.

To start, I created a new Jekyll site by using `jekyll new myblog` in the terminal as per the [Jekyll documentation][4]. I checked out how it looked by running `jekyll serve` inside the newly created code directory and directed my browser to `http://localhost:4000/`. I had a working static website! Unfortunately the standard Jekyll theme seemed a bit bland and is only equipped with the bare essentials for responsive design. Fortunately Jekyll is amazingly flexible and customizable thanks to YAML front matter processing and flawless HTML integration! Even better [there is an amazing selection of diverse themes][5] to make any Jekyll site a bit more modern and fun.

## Paradox of Choice

I soon learned that since Jekyll is so flexible, every Jekyll theme I looked at was widely different than the last. Some used front-end frameworks and some not at all. Some themes used Sass for styling and some just used plain CSS. All the themes also had varying degrees of available features like sidebars and responsive galleries. In order to make responsive design much easier for myself, I wanted a Jekyll theme that utilized a framework like Bootstrap or Foundation. After considerable shopping around and trying on different themes I settled with [Feeling Responsive][6]. I liked the bright and inviting design and it's use of [Foundation Framework][7] lends cool features like [accordion menus][8] and <a href="#" data-reveal-id="videoModal">reveal modals</a>. Foundation's grid system gave me confidence my site would look good on any screen size. Also MIT license FTW.

## Making it Mine

Now that I had settled on a theme I needed to add my own content and put my own touches on the design. I started by adding my portrait as a logo along with cleaning up the navbar and front page since my site would be much simpler than the template's example. Soon I noticed the [icon set][9] that came integrated into the Feeling Responsive theme left much to be desired. I preferred [Font Awesome][10] for it's massive selection of icons but I had to figure out how to integrate it into my theme's existing Sass.

Good thing Sass is easy to work with! In order to get Font Awesome working on my site I just had to copy it's `fonts/` and `scss/` directories into my site's respective directories. After that it was as simple as adding `@import "font-awesome/font-awesome";` to my site's primary Sass file and replacing the old icon HTML with awesome ones. <i class="fa fa-smile-o"></i>

Later I thought it would make sense to use [Foundation's tab feature][11] to switch between content on my [contact page][12]. I couldn't find documentation for the feature on Feeling Responsive's site so I looked in the Sass files and found that the feature was indeed missing. I also noticed that the theme uses an old version of Foundation, 5.5.0, so I would have to download the legacy version when importing the Sass. Once again adding the Sass was fairly easy but since tabbed content is a JavaScript feature I also had to incorporate the JavaScript. After figuring out the correct placements of copy and paste for the JavaScript it was working and my tabs were switching content! The satisfaction I was getting from flipping between content on one page was well worth all the trouble I had just went through.

## Just the Beginning

My site is really sparse on content right now but I am just getting started and Jekyll along with GitHub pushing makes updating, adding and publishing content a breeze. I'm a bit of a perfectionist and there are several things in the back of my mind that are nagging me to be fixed but my site currently meets my initial requirements which makes me very happy! I'm looking forward to filling out my portfolio and learning new things along the way!


## Other Posts About Jekyll
{: .t60 }
{% include list-posts tag='jekyll' %}

<div id="videoModal" class="reveal-modal large" data-reveal="">
  <div class="flex-video widescreen vimeo" style="display: block;">
    <iframe width="1280" height="720" src="https://www.youtube.com/embed/3b5zCFSmVvU" frameborder="0" allowfullscreen></iframe>
  </div>
  <a class="close-reveal-modal">&#215;</a>
</div>

 [1]: https://wordpress.org/
 [2]: http://jekyllrb.com/
 [3]: https://help.github.com/articles/using-jekyll-with-pages/
 [4]: http://jekyllrb.com/docs/quickstart/
 [5]: http://jekyllthemes.org/
 [6]: https://phlow.github.io/feeling-responsive/
 [7]: http://foundation.zurb.com/sites.html
 [8]: {{ site.url }}/blog/archive/
 [9]: https://phlow.github.io/feeling-responsive/assets/fonts/iconfont-preview.html
 [10]: https://fortawesome.github.io/Font-Awesome/
 [11]: http://foundation.zurb.com/sites/docs/v/5.5.3/components/tabs.html
 [12]: {{ site.url }}/contact/
