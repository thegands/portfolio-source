---
layout: page
subheadline: Development
title:  "Deploy a Sinatra App Fast"
teaser: "A guide to deploy a Sinatra app in less than 10 minutes - free!"
breadcrumb: true
tags:
    - ruby
    - sinatra
categories:
    - blog
header: no
image:
    thumb: sinatra-thumb.png
comments: true
---
If you have a Sinatra application that you want to deploy to the web but are unsure of how to go about doing so, you've come to the right place! This article will help you get a Sinatra application online in less than 10 minutes. Your application will have an integrated MySQL database with all the gems and MVC coding conventions familiar to learn.co students starting their [Sinatra Assessment][1]. If you want to get straight to the instructions and skip my spiel, <a href="#deployment-eta--10-minutes">click here</a>.

After learning how to build a Ruby application with Sinatra, I immediately wanted to throw a simple app up on the web and play around with it, maybe even show it off. Being always frugal, I wanted to do this with a reliable and also free of charge hosting service, two things that are not usually provided together. Heroku seemed to be the obvious choice, even [The Ruby on Rails Tutorial][2] suggests [deploying your first Rails app to Heroku][3].

## OpenShift over Heroku
I chose Red Hat's [OpenShift][4] service instead of Heroku for one reason: Heroku's [free tier][5] imposes mandatory idling on your app, forcing it offline 25% of every day. OpenShift's apps will happily serve content all day as long as there are no hiccups in their infrastructure. Don't get me wrong, Heroku provides an amazing service for free. It would not even surprise me if it would have been easier to get a Sinatra app online on Heroku instead of OpenShift. But Heroku's mandatory idling was simply unacceptable for me. Luckily, I thought, OpenShift had a Sinatra QuickStart to instantly deploy a working foundation of a Sinatra app!

Upon using OpenShift's [Sinatra QuickStart][6] I soon discovered that it would only function out of the box with OpenShift's Ruby 1.9 'cartridge'. There was even a [3 month old issue on GitHub][7] for it with no apparent solution. Since I didn't want to get shut out of being able to use fun Ruby 2 features like [keyword arguments][8], I set out to get the Sinatra QuickStart working under OpenShift's Ruby 2.0 cartridge. Turns out all it took was a [simple tweak to the <mark>Gemfile.lock</mark>][10] file! Having figured that out I was ready to get an app online.

## Learning Good Advice the Hard way
After attempting to wholesale copy my code from a previous Sinatra lesson over to my OpenShift application, I learned that the very first thing I read in the [deploying section 1.5][3] of The Ruby on Rails Tutorial was very good advice indeed:
<blockquote>deploying early and often allows us to catch any deployment problems early in our development cycle. The alternative—deploying only after laborious effort sealed away in a development environment—often leads to terrible integration headaches when launch time comes.</blockquote>
The application simply did not work after indiscriminately replacing code and I would have to refactor the QuickStart code in increments. OpenShift's Sinatra QuickStart had separate examples for modular views, models and controllers. Their model example also only had support for PostgresSQL. I wanted a QuickStart that got you started with a completely modular structure and instead of using PostgresSQL, which seemed a bit overkill, I wanted to use MySQL. After tinkering away for a couple days I am finally confident in sharing my work for other's benefit, and thanks to OpenShift's community-driven QuickStarts, you can quickly and easily reap the reward!

## Deployment ETA < 10 minutes
1. Sign up for a [free OpenShift account][11].
2. [Install the OpenShift client tool][12]: `sudo gem install rhc`
3. [Setup rhc][13]: `rhc setup` & follow the prompts.
4. [Create your Sinatra app using my Quickstart][14]: `rhc app create MySinatraApp ruby-2.0 mysql-5.5 phpmyadmin --from-code=git://github.com/thegands/sinatra-mysql.git -e BUNDLE_WITHOUT='development test'` _and be **patient**, it can take several minutes._
5. Visit your newly created application's webpage by clicking the <i class="fa fa-external-link"></i> icon near your application's name in the [OpenShift Dashboard][15]. If everything went according to plan you should see an all white page with the words "Sinatra is up!". Congrats!

## Start Coding!
1. If you used rhc to create your Sinatra application in the previous step, #4, the application code was already cloned to your local drive and you can skip to the next step. Otherwise, clone your OpenShift code repo to your local drive using the address from the application's page on your [OpenShift account dashboard][15]: `git clone <Your application's SSH Source Code address>`
2. Change your directory to the cloned code repo and use bundler to ensure you have all the required gems: `bundle install --without production`
3. Create a migration file: `rake db:create_migration NAME=create_users`
4. Add code to create a users table into the migration file and make any other desired adjustments.
5. Push your changes to OpenShift for deployment:
    5. `git add .`
    5. `git commit -m "First OpenShift push!"`
    5. `git push`

## Troubleshooting and Maintenance
If you added some database functionality to your website through a model class, you may have noticed that your application no longer functions and returns an Internal Server Error (500). That is because even though you created and pushed the migration file, we still need to tell the server to execute it and create the users table in the database:

1. Start a secure shell session with your OpenShift application: `rhc ssh -a MySinatraApp`
2. Change the directory to the code repo: `cd app-root/repo`
3. Execute the rake migration task through bundler: `bundle exec rake db:migrate RACK_ENV=production`
4. Just like that, your website should be up and running again!

<hr>

Maintaining the database can easily be done through your browser by using the phpMyAdmin link and the username+password on your [application's dashboard page][15].

<hr>

If you are getting server errors on your deployed application and not on your copy running locally, you may need to view the OpenShift server logs:

1. Start a secure shell session with your OpenShift application: `rhc ssh -a MySinatraApp`
2. Change the directory to the log directory: `cd app-root/logs`
3. View the application log: `vim ruby.log` \|\| `mysql.log` \|\| `production.log`

<hr>

If your OpenShift application is experiencing gem or dependency issues, first run `bundle install --without production` in your code's local repo and push the changes if your <mark>Gemfile.lock</mark> file changed. If it did not change or it still does not work, create an empty file from your code repo by running `touch ./.openshift/markers/force_clean_build`. Commit and push the changes. Pushing your code with a file named <mark>force_clean_build</mark> in the markers directory forces OpenShift to run a clean bundle install on your application for every push. Running `rhc env set BUNDLE_WITHOUT='development test' -a MySinatraApp` before pushing a force clean will prevent OpenShift from installing unnecessary gems. You only need to set the environment variable once - it will be remembered by the app.

<hr>

If you find any issues you would like to raise with my Sinatra-MySQL QuickStart, please [submit one on GitHub][16]! It is intended to just work and I want to try to make it work for everyone, so input is appreciated!

OpenShift also provides [valuable information about their Ruby application hosting service][17] that is sure to come in handy!

## Show Appreciation?
Did this guide help you get your app up and running? Did it just leave you frustrated and confused? <a href="#comments">Leave a comment</a> with your experience down below!

## Other Posts About Ruby
{: .t60 }
{% include list-posts tag='ruby' %}

 [1]: https://github.com/learn-co-curriculum/sinatra-cms-app-assessment
 [2]: https://www.railstutorial.org/
 [3]: https://www.railstutorial.org/book/beginning#sec-deploying
 [4]: https://www.openshift.com/
 [5]: https://www.heroku.com/pricing#heroku-dyno-free
 [6]: https://hub.openshift.com/quickstarts/118-sinatra
 [7]: https://github.com/openshift/sinatra-example/issues/13
 [8]: https://robots.thoughtbot.com/ruby-2-keyword-arguments
 [9]: https://robots.thoughtbot.com/ruby-2-keyword-arguments
 [10]: https://github.com/thegands/sinatra-mysql/commit/2220224ae75e7037aab99763ff665c34d44c3c71
 [11]: https://www.openshift.com/app/account/new
 [12]: https://developers.openshift.com/en/getting-started-osx.html#client-tools
 [13]: https://developers.openshift.com/en/getting-started-osx.html#rhc-setup
 [14]: https://github.com/thegands/sinatra-mysql
 [15]: https://openshift.redhat.com/app/console/applications
 [16]: https://github.com/thegands/sinatra-mysql/issues/new
 [17]: https://developers.openshift.com/en/ruby-overview.html
