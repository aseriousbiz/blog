---
title: "Tools For Inside the Workshop"
description: "Abbot brings a different style of work to teams, where people build tools for themselves and each other."
tags: [tools, devtools, chatops]
excerpt_image: https://user-images.githubusercontent.com/279389/125671459-9eb8385d-66a0-4010-9d77-49a00f9919f5.jpg
author:
    avatar: https://avatars.githubusercontent.com/u/279389?v=4
    name: pmn
---
 
No two workshops are the same. Every workshop evolves its own blend of wear patterns, scars, and scratches on the floor with use over time. Something else emerges – the tools that the craftspeople make for themselves to use inside the workshop. This collection of purpose-built tools create a special language in the workshop, a dialect unique to what the shop creates; and unique to the collection of people doing the creation.

As people change jobs, they bring the best of the tools they used at their last shop with them. They may not work exactly the same way in the new place; but it makes a great starting point for helping everyone else build faster, more accurately, and with less effort. Some of these tools are bought off the shelf, but often there are tools that are made to fill a specific need quickly and simply.

The combination of these purpose-built tools create a lot of power. One jig, one template, one note may make a small improvement on their own, but over time the combination of the tools helps everyone else work more efficiently and learn how to do things faster inside the shop.

In the physical world, these tools are assembled with a few pieces of whatever is around the shop already: scrap wood, extra metal, tape. In the digital world, we can make our tools with code.


![Many tools on a wall inside a workshop](https://user-images.githubusercontent.com/279389/125671459-9eb8385d-66a0-4010-9d77-49a00f9919f5.jpg)


## Abbot helps you build tools for inside your workshop
It may not be the place where individual work is done, but chat is the workshop for technology companies today. It's where people gather to talk, make decisions, coordinate, and collaborate. We designed Abbot to make building tools for inside this workshop as simple as possible. Because we supply the infrastructure that runs these tools, builders and makers can focus on what’s essential – the logic for their tools. Since Abbot tracks who writes and runs the code, who accesses secrets, where data is stored (and much more); developers are freed from having to write yet another web app to kick off a scheduled job, or from having to keep tabs on a rarely touched system just to make sure things are working.

This allows teams to build the smallest possible units of automation. For example, we have an Abbot skill called `strlen` that returns the length of a string and the number of words it contains. It’s only three lines of code [^1], but we use it a few times a week. **Without Abbot, we would never build a tool like this.** It’s far too much work to build a chat integration for such small things; but because we have made it simple to build these, we have dozens like it. Working this way is incredibly freeing. Instead of opening a Python shell to script something locally, Abbot's REPL [^2] makes it trivial to write, test, and share this code with everyone.


## The Joy of Simple Automation

> “Now we shall begin to see in detail how the rich and complex order of a town can grow from thousands of creative acts. For once we have a common language in our town, we shall all have the power to make our streets and buildings live, through our most ordinary acts. The language, like a seed, is the genetic system which gives our millions of small acts the power to form a whole.” <br><sub>Christopher Alexander, _[The Timeless Way of Building](https://www.patternlanguage.com/bookstore/timeless-way-of-building.html)_</sub>


The collection of these purpose-built tools have an impact greater than the sum of their parts. When new contributors learn how to use the tools in the shop, they’re able to work along the rest of the organization more quickly and confidently. Since Abbot brings those tools to chat, new contributors are exposed to the language of the workshop as soon as they arrive. It’s easy to build these tools, so new contributors can create their own skills and share them without having to ask permission or figure out where to put them. This way everyone can take part in automating away rote tasks and make life better for everyone around them.

We saw this at work at GitHub with [Hubot](https://github.com/github/hubot). Nearly every team at GitHub had their own Hubot skills, which they could call from chat. Hubot was used to deploy branches, query servers, and give each other internet high-fives. What helped make it special was that anyone could contribute because Hubot abstracted away a lot of the hassle of making a chatbot. We've abstracted away even more with Abbot, in order to let people focus purely on the task at hand.

When it’s possible for people to create their own tools internally with a minimum of fuss, more people create them. By doing it through Abbot, teams get a lot for free. Abbot provides a safe sandbox for developers to put their scripts. Since everything is logged and there is a permission system built in, administrators can feel confident about what’s happening on their Abbot installation.

## Minimum Shareable Products
Many of our most used skills started as a quick experiment, often built during a short video call while we explore a problem space. It's easy to grow skills as they get used, and we've found there's no better way to build a skill than to start using it and growing it organically over time.

Because we abstract away nearly all of the infrastructure for running skills, it's easy to share them. Abbot skills work everywhere that Abbot does, so developers don't have to think about whether or not the skill will be run in Slack, Discord, or Microsoft Teams. Copying and pasting code can be annoying, so we built Abbot's [Package Directory](https://ab.bot/packages) very early on. Even simple skills are worth sharing. One of the first skills ever shared in the directory taught Abbot how to [roll dice](https://ab.bot/packages/336298988896518154/roll), for example. We share a lot of the skills we build to help save people time. There's no reason to re-invent the wheel (or the [sparkles](https://ab.bot/packages/seriousdemos/sparkle), for that matter).

Skills installed from the Package Directory are treated the same way as skills built by your team. In fact, you get access to the source code for all skills shared in the directory so you can make any modifications you'd like. In addition, these skills benefit from the same supporting infrastructure as all other Abbot skills. Integrated help, discovery, permissions all work the same as skills you built yourself.

## Collaboration is fun
As you might have guessed, we don't like to have fun at A Serious Business, Inc. Despite that, it's worth mentioning that these skills are a great way to ship a little piece of your team's personality into your workspace. Just like you might have a hammer with a hot pink handle in the shop named "The Smashinator", you can make some fun things for your team to use with Abbot as well. We make heavy use of [custom lists](https://blog.ab.bot/archive/2021/06/22/shipit/) to tell people when we're excited, or to keep track of our favorite robot jokes. There's even an installable package for [pugs on demand](https://ab.bot/packages/aseriousbiz/pug).

It turns out that shipping your sense of humor (or lack thereof) into chat can be a pretty great teambuilding experience. If your team works remotely even some of the time, having some fun inside the shop makes working together a lot more enjoyable.

We've worked to make getting started with Abbot as easy as possible. The next time you find yourself thinking that you wished you had a tool in chat, no matter how small, please install Abbot and give it a try. We're confident you can have your first skill built and working in chat faster than you'd be able to clone and host any bot framework. As always, we're looking forward to your feedback and suggestions on how to improve Abbot. Just say `@abbot feedback` in chat to send it our way!



**Footnotes**

[^1]: [Create a new Python skill](https://ab.bot/skills/create/python) in Abbot and paste this code to have your own `@abbot strlen` command: 
    ```python
    words = len(bot.arguments.split())
    chars = len(bot.arguments)
    bot.reply("Your string is {} characters long ({} words)".format(chars, words))
    ```
[^2]: [read-eval-print-loop](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)



