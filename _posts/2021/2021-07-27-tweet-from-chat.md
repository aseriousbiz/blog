---
title: "Tweet from Chat"
description: "Never tweet from the wrong account again. Use the Abbot tweet skill to manage your company or team's Twitter account from chat."
tags: [twitter]
excerpt_image: https://user-images.githubusercontent.com/19977/127073042-60c19aee-62d2-4bea-ad6c-7028395defcc.png
author:
    avatar: https://2.gravatar.com/avatar/cdf546b601bf29a7eb4ca777544d11cd?s=160
    name: haacked
---

If you manage more than one Twitter account, whether it's for a project or a company's social media presence, it's safe to say that a huge fear is tweeting something from the wrong account. I've done it. Maybe you have. Hopefully not as bad this person:

![Failed tweet from a StubHub account: Thank fuck it's Friday! Can't wait to get out of this stubsucking hell hole.](https://user-images.githubusercontent.com/19977/127071611-68f7610e-eb7c-49ad-93da-0ce430f7ac5a.jpg)

_(image from [Gawker](https://gawker.com/5949503/the-social-media-director-at-stubhub-is-not-having-a-good-night))_

Look, we've all felt the same way, but it still sucks to tweet from the wrong account. And it's really easy to do if you're using your phone to manage multiple accounts. However, with Abbot, there's a better way.

## Tweet from chat with Abbot

Over here at A Serious Business, Inc. we manage our official [@getAbbot](https://twitter.com/getAbbot) twitter account in Slack using [Abbot's `tweet` skill](https://ab.bot/packages/aseriousbiz/tweet).

This is based on our experiences at GitHub where we would use a chat bot (Hubot) to tweet from the official GitHub account. There's a lot of benefits to this approach:

1. Anyone with access to the chat room can tweet from the official account.
2. Anyone can learn how to tweet from the official account by looking at chat logs.
3. The account credentials do not need to be shared.
4. Since the account is managed from chat, and credentials are not shared, it's unlikely someone will accidentally tweet from that account by accident on their phone.
5. As we grow, we can lock down access to the skill using [Abbot's access controls](https://youtu.be/6NHMyyWZtrU) as needed.

## Let's see it in action!

![Screen Shot of @haacked running the tweet skill and Abbot creating a tweet](https://user-images.githubusercontent.com/19977/127073042-60c19aee-62d2-4bea-ad6c-7028395defcc.png)

Here's a screenshot of me using Abbot to create a tweet. It's not a great tweet, [but it's out there](https://twitter.com/getAbbot/status/1419805425101279235).

You can also use Abbot to have your Twitter account follow and unfollow other accounts as well as reply to, like, or retweet a tweet.

## How do I start?

Starting is easy. If you don't have Abbot installed, it's a [couple of clicks to get it installed](https://ab.bot/login) (assuming you have permission to install an app into Slack, Discord, or Teams).

Once you have Abbot installed, you can create the `tweet` skill by clicking the "Install Package" button in the [package listing for the `tweet` skill](https://ab.bot/packages/aseriousbiz/tweet). It's really that easy!

## Authorizing Abbot to manage the Twitter account

Once you have the `tweet` skill, you will need to Authorize Abbot so that it can tweet on behalf of your Twitter account by running the following command:

![@abbot tweet auth](https://user-images.githubusercontent.com/19977/127073610-ce7fbe79-1830-42bf-bc80-97dad5874210.png)

After running this command, Abbot responds with a URL you can use to authorize Abbot to access your Twitter account. That'll take you to a page that looks something like this:

![Page used to authorize Ab.bot](https://user-images.githubusercontent.com/19977/127073714-d9264cbf-6b5f-4b97-8abf-33363e059950.png)

Make sure that the domain in the address bar is `api.twitter.com`. Once you authorize the app, Twitter will show you a seven-digit pin.

![Screenshot of an access PIN: 584780](https://user-images.githubusercontent.com/19977/127074678-9dba34e9-f737-4590-9c1f-f211ec03dd1f.png)

You'll need to give this PIN to Abbot to complete the authorization process.

![@abbot tweet auth 584780](https://user-images.githubusercontent.com/19977/127074789-be348bb3-4012-493b-b383-f4edbbe65abb.png)

And with that, you're all set. Note that authorization is per chat room. That makes it easy to have one room manage one Twitter account while another room manages another. To find out which twitter account you're running in a room, run `@abbot tweet auth user`. Happy tweeting!

## Feedback?

You can find the source code for [this skill here](https://github.com/aseriousbiz/abbot-skills/blob/main/skills/tweet.py). If you have feedback on the skill, [log an issue](https://github.com/aseriousbiz/abbot-skills/issues).