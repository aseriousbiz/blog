---
title: "Updating the CascadiaJS live stream with Abbot"
description: "CascadiaJS uses Abbot patterns to power its live feed. See how to use Patterns in this post"
tags: [patterns,cascadiajs,abbot]
excerpt_image: 
author:
    avatar: https://avatars.githubusercontent.com/u/279389?s=400&u=74e6e598a2bf9d9889fbb771c542c508bf270e36&v=4
    name: pmn
---

If you attended CascadiaJS 2021, you may have noticed live updates on the website that came from Cascadia's Discord server. 

The CascadiaJS team did this using [Abbot](https://ab.bot/). They installed Abbot into their Discord server, and created a command called “channel-sync” that listens for messages in a room they’ve set up. Any time someone sends a message, Abbot takes that message and some metadata about who sent it and posts it to an external server in order to update the live feed.

You could build something like this yourself. Start by [signing up for an Abbot account](https://ab.bot) and connecting it to your chat server (Abbot supports Slack, Discord, or Microsoft Teams). 

CascadiaJS's `channel-sync` skill sends every message that Abbot sees out to another system. Let’s build something that is a little less noisy. Once you're done, you’ll have all the tools you need to build anything you’d like.

Let's build a simple script that listens for people asking if a site is down (__“is www.aseriousbusiness.com down?”__). If Abbot is in the room when someone asks the question, it can try to connect to the server and let the user know if the site is up or down. 

Since this was done for a JavaScript conference, let's build our script using JavaScript. Log in to Abbot and create a new JavaScript skill called `website-checker`. Copy and paste the code below into your skill:

```javascript
  const axios = require('axios');

  let website = bot.arguments; // bot.arguments contains the user-provided text
  if (bot.isPatternMatch) {    // this will be true if it's the result of a pattern match. Otherwise, it's a chat message.
    website = bot.tokenizedArguments[1].toString(); // Abbot tokenizes arguments to make it easier to ahandle them.
  }
  
  await axios.get(website)
    .then(function (response) {
        // handle success
        bot.reply(`${bot.from} it's just you.. it looks okay to me...`)
      })
      .catch(function (error) {
        // handle error
        bot.reply(`it's not just you ${bot.from}, i got this error when i tried: \n ${error}`)
      });
```

Test the skill in the console by entering the domain of a website you want to check. In the console, you will need to include `https://` in front of the domain name. 

Once you’ve verified that the skill works, add a Pattern by clicking the link that says “Manage skill patterns” on the left side of the screen. Click “Create Pattern”, and give the new pattern a descriptive name like “url pattern”. To save time, pick a pattern type of “Regular Expression” and use this regular expression: 

`is (<)?(http:\/\/|https:\/\/)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b([-a-zA-Z0-9()@:%_\+.~#?&//=]*)(|.*?)(>?) down`

This Pattern is made a little more complicated by all the ways chat platforms might send us the URL, for now you'll just have to trust that it works. You can see for yourself by using the built-in tester. Try a phrase like “is google.com down?” to make sure it matches.

Once this is enabled, jump into any chat room that has Abbot in it, and try asking if a site is up or down. Go on, we’ll wait for you to try it! 

The pattern above registered a listener (which we call a Pattern) and attached it to the `website-checker` skill you created. You can see all the patterns that Abbot is configured to use by visiting [https://ab.bot/skills/patterns/all](https://ab.bot/skills/patterns/all). Now any time someone asks if a website is down in a room with Abbot, it will do all the work of checking. 

Patterns can contain any kind of regular expression. [You can read more about them here]({% post_url 2021/2021-11-03-introducing-patterns %}).



&nbsp; 
