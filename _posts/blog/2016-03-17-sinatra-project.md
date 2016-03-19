---
layout: page
subheadline: Development
title:  "My Sinatra Project"
teaser: "My first major Ruby project is a Reddit clone dubbed Toast-It"
breadcrumb: true
tags:
    - ruby
    - sinatra
categories:
    - blog
header: no
image:
    thumb: toast-it-thumb.png
comments: true
---
I chose to make a Reddit-like website for my first Ruby web application project. I love reddit and since I visit the site more than once a day I thought a Reddit-like website would be a good starting project to demonstrate basic CRUD procedures in Ruby. I named my application [Toast-It][1] in homage of the original. My initial intention was to make a relatively simple website but as I started to build the project I was increasingly critical of the UI and design aspects. One thing led to another and my project became something of a monstrous achievement but I am incredibly satisfied with the end result! I tried to encompass almost all of what I have learned in the way of web development and it is by far my most ambitious web project to date.

Toast-It features a login system to prevent unauthorized users from making changes to the site. I used the [Materialize CSS][2] front end framework to make it look pretty and RedHat's [OpenShift][3] service to deploy a functional demo of my project. I always thought compiled assets look so professional so I chose to use a [Yeoman][4] [Sinatra generator][5] to help me get CSS and JavaScript compiling. I soon noticed that the yeoman generated project used a templating engine named [Haml][6] instead of ERB. Even though my project still supported ERB, after looking at it's documentation, I decided to use Haml instead. Haml, like SASS, uses indentation to replace closing tags which just by itself makes HTML look way cleaner and easier to read, not to mention easier to write. Haml also uses syntax inspired by CSS and Ruby to condense HTML code even further. The result is something that immediately caught my attention because of how good it looks in comparison to vanilla HTML and how easy it is to learn and use. Try using Haml for your next Ruby web app!! You will not regret it.

## Front End is Hard

During my development process I came to several realizations. The primary one being that, for me, front end development is hard! I had an exceedingly difficult time getting simple things to work. Block elements wouldn't play nice with inline elements. Changes I made to my stylesheet would soon cause perplexing behavior to related elements after making layout changes causing me a good deal confusion. Trying to keep the website responsive also proved a challenge with elements overlapping when narrowing the view. The situation at the beginning probably wasn't being helped by Haml. Trying to implement a Materialize feature took 15 minutes of attempting to rewrite HTML instead of the usual copy/paste. The Materialize front end framework did help out big time, the final website is something I would have no hope to build in it's absence. Eventually, I had things mostly figured out, but I think I'm not really cut out for front end work; the back end coding is where I got most of my enjoyment from this project.

If you have trouble with the front end like me, I really recommend checking out [CodePen][7] and giving it a try when playing around with a new framework or markup language. CodePen will update the live view with any changes you make to the code plus supports Sass, CoffeeScript and Haml, which is great for getting familiar with their syntax. CodePen allowed me to easily create a mockup of the concept I had for my site. The [result is really close][8] to the final design of Toast-It because a lot of the code was just copied out of my pen and pasted into my project!

## Active Record Rabbit Hole

My favorite aspect of Ruby CRUD application development is the database and the logic behind it. It's pretty amazing that Active Record can do so much for you from a simple database table and a declared but empty Ruby class. I have to suspect that half the magic of Ruby on Rails that I hear so much about comes directly from Active Record. The biggest detriment to all this magic is wading through documentation. Active Record does *so* much that trying to figure out a simple function can become a difficult journey through stackoverflow questions and answers, especially for a complete beginner like me. I endured a particularly difficult journey with Active Record's count method.

## Array#count != ActiveRecord#count

One of the primary features of Reddit is the ability to upvote or downvote a post you like or dislike. Since this really is a defining aspect of Reddit, I wanted my site to feature upvoting and downvoting. Actually implementing was much harder than I thought it would be, mostly because I was not sure how to properly implement such a feature using a database. I settled on using a `Score` model with database objects having a one to many relationship with `User`. Each instance of a `Score` would also have a one to many relationship to either a `Topic` or `Comment` using a [polymorphic association][9]. Every instance of `Score` would be also be required to have a `score_instance.liked` attribute of either `true` or `false`. When the topics are being displayed to a user of my application, each topic's and comment's score must be calculated by taking the difference of the positive and negative votes, or likes, associated with that instance of `Topic` or `Comment`.

I was having trouble getting the `Score` Active Record class to tell me the number of instances of `Score` that had a liked attribute of `true`. Looking at the [Array#count][10] documentation I thought `Score.all.count { |s| s.liked }` would produce the desired result. After all, `Score.all` produces an array, right? But no matter what kind of scores I had available to my model, it would always tell me the number of `Score.count` instead of the ones that actually had a liked attribute of `true`. It drove me absolutely bonkers but eventually I learned that it was because I was not using [Array#count][10] at all! I was using [ActiveRecord#count][11] which was counting the `Score` instances that did not have a liked attribute of `nil`, which was all of them! This is distinctly different than [Array#count][10] method because [ActiveRecord.count { \|item\| block }][11] uses the [COUNT SQL statement][12] to produce the return value. This was a good lesson, I am now more aware of the kind of class I am working with and the kinds of methods it could possibly have available.

## Counting Scores is Bad Design

I am quite proud of the score counting code I ended up writing, if only because I had to use a bit of simple math to figure it out:

~~~
class Topic && class Comment
  def score
   2 * Score.where(post: self, liked: true).count - self.scores.count
  end
end
~~~

This is essentially subtracting the total number of scores belonging to the topic from twice the amount of the topic's scores with a liked value of `true`. I like this way because it only makes my application count individual score attributes once for each topic/comment. I love math so even figuring out a simple equation like this was fun for me. Let's start with the way we would normally think to count scores:

liked - disliked = score

We also know:

liked + disliked = count of scores(votes)

There are some similar looking variables there... Are you thinking what I'm thinking? Let's add them together!

+liked -disliked = +score  
<u>+liked +disliked = +count</u>  
liked + liked = score + count

or

2*liked = score + count

If we subtract count from both sides we get:

2*liked - count = score

If you're still reading, I think you can agree, math is fun!

In retrospect, that bit of code I'm so proud of will be one of the first things that I will refactor. It should not be hard to give the topics and comments table a new integer column named score and push logic into the models to increment or decrement the `Topic` or `Comment` instance's score value in the table. That way the application does not have to calculate the score numerous times on each page request, it just grabs the score value from the table the same way the title or other content is grabbed from the database.

## Still Not Good Enough

Even though my project works as intended and ended up exceeding most of my initial expectations, I am still not satisfied with it so I still consider it a work in progress. Users of Reddit can reply to other people's comments and Reddit effectively nests and orders comments. I had a hard time conceptualizing how I would go about making that feature work but I believe all it would take is an `has_many :comments` addition to the `Comment` class/table. However, that introduces the problem of nesting comments and creating separate views for comment branches that are nested too deeply to display properly on all screen sizes. All of this seemed like a headache so I opted to skip that feature for now and just allow comments to be made on topics without any nesting.

Another aspect of my application that I very much want to improve is the route that takes votes and creates instances of `Score`s. It is a terrible mess of nested if and else statements that I think could be refactored with a case statement or even better maybe even logic in the model. It was hard for me to figure out and the mess it turned out to be reflects that. It is fully functional though and for now that is all that counts!

As the site is, no matter how many topics are posted on the site, they are all displayed on one page. This isn't a problem when there are only a handful of topics but it will be troublesome when there are hundreds of topics. Pagination is definitely a much needed feature but I think I might wait until I know how to make an infinite scrolling page with AJAX or similar to add that feature.

There is also a search bar I lazily left in the Toast-It navbar that does not do anything at all, it is not even connected to a form. Ideally my app will have a simple search feature to display the sorted results of a keyword search.

I could keep going with Toast-It's current shortcomings but it seems like the more I write, the more things I think of! The site was a ton of fun to work on though so I'm looking forward to making these improvements and more in the future!

## Other Posts About Ruby
{: .t60 }
{% include list-posts tag='ruby' %}

 [1]: http://ruby-thegands.rhcloud.com/toast-it
 [2]: http://materializecss.com/
 [3]: https://www.openshift.com/
 [4]: http://yeoman.io/
 [5]: https://github.com/therubyc/generator-sinatra-bootstrap
 [6]: http://haml.info/
 [7]: http://codepen.io/
 [8]: http://codepen.io/thegands/pen/ZWpOwd
 [9]: http://guides.rubyonrails.org/association_basics.html#polymorphic-associations
 [10]: http://ruby-doc.org/core-2.2.0/Array.html#method-i-count
 [11]: http://api.rubyonrails.org/classes/ActiveRecord/Calculations.html#method-i-count
 [12]: http://www.techonthenet.com/sql/count.php
