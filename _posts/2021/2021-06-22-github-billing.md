---
title: "Understand your GitHub Usage Cost with Abbot"
description: ""
tags: [github]
excerpt_image: https://user-images.githubusercontent.com/19977/122844477-9922af00-d2b6-11eb-9c1e-760759e071ab.png
author:
    avatar: https://2.gravatar.com/avatar/cdf546b601bf29a7eb4ca777544d11cd?s=160
    name: haacked
---

When using services that charge based on usage, it is helpful to have periodic reminders of how much you've used so far. For example, if you use GitHub, there are monthly limits to the usage of Actions, Packages, and Storage before you incur extra costs. Wouldn't it be useful to have a daily or weekly reminder of your current usage?

This is where an Abbot skill can come in handy!

In this post, we'll cover a new `github` skill. There's so much a `github` skill could do, but we'll start with one simple use case, reporting on your current billing situation. If you want to jump to the punch line and just start using it, you can install it from the package directory: https://ab.bot/packages/aseriousbiz/github.

## Setting up the skill

After you install the skill, make sure to save it.

In order to use the skill, you will need to [set up a Personal Access Token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token). If you want a billing report for an organization, make sure the token has `admin:org` permissions. If you want to report on a user account, make sure it has the `user` permission.

Now you'll create a secret for the skill you just created named `GitHubToken` and for the value, paste in the personal access token.

## Running the skill

The skill needs to know which organization you want a billing report for. You can tell Abbot to set a default:

```bash
@abbot github default my-github-org
```

Then you can run the `billing` sub-command.

```bash
@abbot github billing
```

And you should get something like the following:

![Response showing information about GitHub Actions, Packages, and Storage usage.](https://user-images.githubusercontent.com/19977/122844477-9922af00-d2b6-11eb-9c1e-760759e071ab.png)

All this information is being pulled by the [GitHub Billing API](https://docs.github.com/en/rest/reference/billing).

## Scheduling the skill

You can call the skill any time you want from chat. But some may want a regular reminder reported to a chat room. This is simple to set up with Abbot.

```bash
@abbot schedule github Billing Report from GitHub
```

This creates a [scheduled trigger](https://docs.ab.bot/guides/triggers/#scheduled-triggers) to call the skill and respond to the current room. Don't worry, the schedule isn't active yet.

![Abbot creates a scheduled trigger](https://user-images.githubusercontent.com/19977/122844905-780e8e00-d2b7-11eb-8bed-d1c1faed4342.png)

When creating a trigger, the `schedule` skill returns a URL to set up the actual details for the schedule such as when it should run and which arguments to pass to it.

When you visit the triggers page, you can see the set of all triggers attached to the skill. In the following screenshot, there's only one - the one we just created.

![Scheduled trigger](https://user-images.githubusercontent.com/19977/122844909-78a72480-d2b7-11eb-9098-bf726caf8ff2.png)

Click "Edit" to edit the scheduled trigger.

![Editing the schedule](https://user-images.githubusercontent.com/19977/122844910-78a72480-d2b7-11eb-9112-e92a6ba8766a.png)

Be sure to pass in "billing" as the arguments to the skill and set the schedule to whatever frequency you want. In the screenshot, you can see I set it to run every day at 9 AM PST. Now you have a periodic reminder for how much you're spending on GitHub.

## Next Steps

There's probably a lot more you'd like a GitHub skill to do. I've made the [source code for this skill available on GitHub](https://github.com/aseriousbiz/abbot-skills/blob/main/skills/github.cs) in case anyone wants to collaborate on it and improve it. And if there are other use cases like this that you're interested, let us know!
