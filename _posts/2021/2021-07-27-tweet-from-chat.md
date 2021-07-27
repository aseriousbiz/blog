---
title: "Tweet from Chat"
description: "Never tweet from the wrong account again. Use the Abbot tweet skill to manage your company or team's Twitter account from chat."
tags: [twitter]
excerpt_image: https://user-images.githubusercontent.com/19977/127073042-60c19aee-62d2-4bea-ad6c-7028395defcc.png
author:
    avatar: https://2.gravatar.com/avatar/cdf546b601bf29a7eb4ca777544d11cd?s=160
    name: haacked
---

If you manage more than one Twitter account, whether it's for a project or a company's social media presence, you live with THE FEAR. That nagging fear that takes up residence in the back of your mind. The fear that one day, you *will* tweet something inappropriate from the wrong account. I've done it. Maybe you've done it. But lets all hope it's not as bad as what this person did:

![Failed tweet from a StubHub account: Thank Frank it's Friday! Can't wait to get out of this stubsucking hell hole.](https://user-images.githubusercontent.com/19977/127176682-1410a4e4-e092-48b0-9653-b120f5e0d147.jpeg)

_(image from [Gawker](https://gawker.com/5949503/the-social-media-director-at-stubhub-is-not-having-a-good-night))_

Hey, who hasn't thanked...Frank...for Friday? Even so, that's a tweet for your personal account, not your employer's. This kind of mistake is really easy to make if you use your phone to manage multiple Twitter accounts. Fornutately, there's a better way with Abbot!

## Tweet from chat with Abbot

Over here at [A Serious Business, Inc.](https://www.aseriousbusiness.com/) we avoid these mistakes because we manage our very serious and very official [@getAbbot](https://twitter.com/getAbbot) twitter account in Slack using [Abbot's `tweet` skill](https://ab.bot/packages/aseriousbiz/tweet).

This is based on our experiences at GitHub where we used a chat bot (Hubot) to tweet from the official GitHub account. There's a lot of benefits to this approach:

1. Anyone with access to the chat room can tweet from the official account.
2. Anyone can learn how to tweet from the official account by seeing how others do it in chat.
3. The account credentials do not need to be shared.
4. Since the account is managed from chat, and credentials are not shared, it's unlikely someone will accidentally tweet from that account by accident on their phone.
5. As we grow, we can lock down access to the skill using [Abbot's access controls](https://youtu.be/6NHMyyWZtrU) as needed.

In fact, when I tweet about this blog post, I'll use the skill. So meta!

## See it in action

![Screen Shot of @haacked running the tweet skill and Abbot creating a tweet](https://user-images.githubusercontent.com/19977/127073042-60c19aee-62d2-4bea-ad6c-7028395defcc.png)

Here's a screenshot of me using Abbot to create [a tweet with one of Abbot's internationally acclaimed jokes](https://twitter.com/getAbbot/status/1419805425101279235). (For those who want more of Abbot's hilarity, try `@abbot joke` after installing Abbot.) We really are lucky Abbot's chosen to stay with us rather than take off on their planned comedy tour.

Creating tweets is only the tip of the iceberg. The `tweet` skill supports following and unfollowing other accounts. It can also reply to tweets, like a tweet, or retweet a tweet. All your tweeting needs are handled by this skill.

## How do I start?

Starting is easy. If you don't have Abbot installed, it's a [couple of clicks to get it installed](https://ab.bot/login).

Once you have Abbot installed, you can install the `tweet` skill by clicking the "Install Package" button in the [package listing for the `tweet` skill](https://ab.bot/packages/aseriousbiz/tweet). It's really that easy!

## Authorizing Abbot to manage the Twitter account

Once you have the `tweet` skill, you will need to authorize Abbot so that it can tweet on behalf of your Twitter account by running the following command:

![@abbot tweet auth](https://user-images.githubusercontent.com/19977/127073610-ce7fbe79-1830-42bf-bc80-97dad5874210.png)

After running this command, Abbot responds with a URL you can use to authorize Abbot to access your Twitter account. That'll take you to a page that looks something like this:

![Page used to authorize Ab.bot](https://user-images.githubusercontent.com/19977/127073714-d9264cbf-6b5f-4b97-8abf-33363e059950.png)

Make sure that the domain in the address bar is `api.twitter.com`. Once you authorize the app, Twitter will show you a seven-digit pin.

![Screenshot of an access PIN: 584780](https://user-images.githubusercontent.com/19977/127074678-9dba34e9-f737-4590-9c1f-f211ec03dd1f.png)

You'll need to give this PIN to Abbot to complete the authorization process.

![@abbot tweet auth 584780](https://user-images.githubusercontent.com/19977/127074789-be348bb3-4012-493b-b383-f4edbbe65abb.png)

And with that, you're all set. Note that authorization is per chat room. That makes it easy to have one room manage one Twitter account while another room manages another. To find out which twitter account you're running in a room, run `@abbot tweet auth user`. Happy tweeting!

## Feedback?

You can find the source code for [this skill here](https://github.com/aseriousbiz/abbot-skills/blob/main/skills/tweet.py). If you have feedback on the skill, [log an issue](https://github.com/aseriousbiz/abbot-skills/issues). If you manage more than one Twitter accounts, you really should give this a try!
