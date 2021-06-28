---
title: "Manage GitHub issues in chat with Abbot"
description: "Chat can be a great place to discuss issues and what to do about them. But why not go further and triage issues and assign them from chat? With the github skill, you can do all that and more! And the skill is customizable to fit your needs."
tags: [github,issues]
excerpt_image: https://user-images.githubusercontent.com/19977/123681438-b26bb400-d7fe-11eb-99bf-b73d8e47ba56.png
author:
    avatar: https://2.gravatar.com/avatar/cdf546b601bf29a7eb4ca777544d11cd?s=160
    name: haacked
---

An important Abbot benefit is it brings the ability to _do work_ where discussion _about work_ is already happening.

For example, if you're in a discussion about an experimental branch of code, someone might suggest deploying it to a test environment. You can use the [`deploy` skill](https://ab.bot/packages/aseriousbiz/deploy) to deploy right away. Even better, everyone in the discussion sees that you did it and how you did it. It's a powerful shared context.

What about when you're discussing an issue in chat? Wouldn't it be nice to be able to assign it to a user in chat? Yes it would. And now, there's a [skill for that](https://ab.bot/packages/aseriousbiz/github), the `github` skill.

## See it in action

Every `github` command requires that you specify a repository. But that can be tedious and Abbot is all about reducing tedium. So let's set a default repository.

```bash
@abbot github default is aseriousbiz/blog
```

This sets the repository `aseriousbiz/blog` as the default repository for the current room. With that, we can triage issues.


```bash
@abbot github issue triage
```

![Screenshot showing the github triage and assign commands](https://user-images.githubusercontent.com/19977/123681438-b26bb400-d7fe-11eb-99bf-b73d8e47ba56.png)

This lists up to 20 unassigned and opened issues in the specified (or default) repository.

_Why twenty? It's a manageable number to list in chat. But the real reason is a limitation in the client library we're using has a design flaw and [it's this guy's fault](https://twitter.com/haacked/status/1408521455470403585)._

If you want to triage a different repository, just specify the repository.

```bash
@abbot github issue triage for haacked/haacked.com
```

_Note that the preposition `for` is optional here, but it feels natural to me._

Let's drill into a specific issue.

```bash
@abbot github issue #123
```

_Note that the `#` sign is optional here, but it doesn't feel right to me to omit it when discussing GitHub issues._

Now after a bit of disscussion in chat, we've decided to assign it. We can assign it to a GitHub username.

```bash
@abbot github issue assign #123 to haacked
```

You can also assign it to a chat user! This has the benefit of mentioning the user at the same time the issue is assigned to them.

```bash
@abbot github issue assign #123 to @phil
```

If Abbot doesn't know @phil's GitHub username, the skill will ask for it. You can set it like so.

```bash
@abbot github user @phil is haacked
```

And if GitHub knows your own GitHub username, you can ask GitHub for issues assigned to yourself.

```bash
@abbot github issues assigned to me
```

Again, this is limited to twenty.

## What about GitHub's Slack Integration

The `github` skill is a nice complement to [GitHub's Slack Integration](https://slack.github.com/). I recommend installing both! There's some overlap between the two, but one nice thing about the `github` skill is you have access to the source code and can easily tweak it to fit your own workflows. And it's easy to map your GitHub user to your chat user.

Also, the `github` skill works in Discord and Microsoft Teams!

## Using an alias to make it shorter

The `github` skill will grow to do a lot. So some of the commands can start to get long. But if you use them often, you can use Abbot's `alias` feature to make things a little more ergonomic. For example, if GitHub is the primary place where you manage issues, you could do this:

```bash
@abbot alias add issue github issue
@abbot alias add issues github issues
```

Now you can omit the `github` part when managing issues:

```bash
@abbot issue #123
@abbot issues assigned to @shana
@abbot issue triage
```

## Lets make it better, together!

If you want to try this out, go to [https://ab.bot/](https://ab.bot/) and install Abbot. Then visit our package directory and [install the `github` skill](https://ab.bot/packages/aseriousbiz/github). That's it!

Abbot does not yet have GitHub integration for source control, but we [created a repository](https://github.com/aseriousbiz/abbot-skills) for some of our packaged skills to make it easy to collaborate with others. So if you have some improvements to make to the skill, send us a pull request! Or log an issue if you have improvements you'd like to see.
