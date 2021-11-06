---
title: "Introducing Signals"
description: "Signals make it possible for Abbot skills to trigger other Abbot skills."
tags: [signals,abbot]
excerpt_image:
    url: https://user-images.githubusercontent.com/19977/140558101-23af87e2-3910-4962-bf3f-331f3950529b.jpeg
    title: Abbot, are you out there? Image by José Francisco Salgado Licensed under CC BY 3.0 https://creativecommons.org/licenses/by/3.0/deed.en
    alt: Four large antennas with a starry sky as the backdrop
author:
    avatar: https://2.gravatar.com/avatar/cdf546b601bf29a7eb4ca777544d11cd?s=160
    name: haacked
---

Signals are a new feature of [Abbot](https://ab.bot/) that enable you to compose skills together to create interesting automated workflows. Skill in Abbot can listen for and respond to signals raised by other skills. This provides a loosely coupled way for skills to trigger other skills within Abbot.

![Four large antennas with a starry sky as the backdrop](https://user-images.githubusercontent.com/19977/140558101-23af87e2-3910-4962-bf3f-331f3950529b.jpeg "Abbot, are you out there? Image by José Francisco Salgado Licensed under CC BY 3.0 https://creativecommons.org/licenses/by/3.0/deed.en")

For those new to Abbot, a skill is a piece of code that runs in response to a user's command in chat. For example, you might use [the tweet skill](https://blog.ab.bot/archive/2021/07/27/tweet-from-chat/) to tweet from chat like so:

```bash
.tweet Wow, Abbot is really cool and you all should try it out!
```

In this example, `tweet` is the command which maps to the name of the `tweet` skill. A skill can also respond to a web hook or be scheduled to run, but it always responds back to chat.

Skills are great for single-purpose commands like this, but the code for a skill can get a bit unwieldy as the skill takes on more features. Instead of a monolithic skill, you can break a skill into smaller pieces and compose them together using Signals. When a skill raises a signal, any other skills that are subscribed to that signal will run.

## How to use Signals

To raise a signal, call `bot.signal` (or `await Bot.SignalAsync` in C#) in your skill code and provide a signal name and arguments. For example, suppose we have a skill named `outage-handler` that can respond to incoming HTTP requests. Perhaps these requests come from a site monitoring service when an outage is detected. It might look like the following:

```csharp
// C# example of raising a signal

if (Bot.IsRequest && Bot.Request.IsJson) {
    var message = await Bot.Request.DeserializeBodyAs<PagerMessage>();
    await Bot.SignalAsync("outage-notification-received", message.Description);
}
```

```python
# Python example of raising a signal

if bot.is_request and bot.request.is_json:
    message = bot.request.json()
    bot.signal("outage-notification-received", message.description)
```

```js
// JavaScript example of raising a signal

if (bot.isRequest && bot.request.isJson) {
    message = bot.request.json()
    await bot.signal("outage-notification-received", message.description);
}
```

_Note that signal names follow the same format as skill names, a single word or set of words separated by a dash. For example, `outage-notification-received` is a valid signal name while `outage notification` is not._

In the above example, when the skill receives an incoming request, it raises a `outage-notification-received` signal and then completes. Any skills that are subscribed to the `outage-notification-received` signal will run asynchronously. Those skills will receive information about the incoming signal in the signal event property:

* `bot.signalEvent` (JavaScript)
* `bot.signal_event` (Python)
* `Bot.SignalEvent` (C#)

Let's look at a skill that listens for a `outage-notification-received` signal and does something in response. We'll call it `outage-pager`. In order to subscribe to a signal, go to the skill editor in Abbot and click "Manage signal subscriptions" in the side bar. In this example, the skill should subscribe to the `outage-notification-received` signal.

```csharp
// C# example of receiving a signal

if (Bot.SignalEvent is {} signalEvent) {
    await Bot.ReplyAsync($"Oh no! An outage! Received signal `{signalEvent.Name}` with arguments `{Bot.Arguments}.`");
    await Bot.ReplyAsync($"Signal raised by {signalEvent.Source.SkillName}.");
    // Code goes here to page the person on call with details about the incident.
}
```

```python
# Python example of receiving a signal

if bot.signal_event is not None:
    bot.reply(f"Oh no! An outage! Received signal `{bot.signal_event.name}` with arguments `{bot.args}`.")
    bot.reply(f"Signal raised by ${bot.signal_event.source.skill_name}.")
    # Code goes here to page the person on call with details about the incident.
```

```js
// JavaScript example of receiving a signal

if (bot.signalEvent) {
    await bot.reply(`Oh no! An outage! Received signal \`${bot.signalEvent.name}\` with arguments \`${bot.args}\`.`);
    await bot.reply(`Signal raised by ${bot.signalEvent.source.skillName}.`);
    // Code goes here to page the person on call with details about the incident.
}
```

When a skill receives a signal, the arguments are populated with the signal arguments. You can also interrogate the signal to get information about the source of the signal as the above examples show.

A skill that responds to a signal can itself also raise a signal, creating a chain of signals. For example, at the end of the `outage-pager` skill, we might raise another signal `log-incident`. A skill that is subscribed to the `log-incident` signal can then log the incident in an issue tracker.

When a skill is part of a chain of signal, you can always access the root source of the signal chain. In this example we respond with information about what initiated the signal chain by looking at the root signal source.

```csharp
// C# example of examining the root source of a signal chain

if (Bot.SignalEvent is {} signalEvent) {
    var rootSource = signalEvent.RootSource;
    var reason = rootSource.IsPatternMatch
        ? $"called via {rootSource.Pattern}"
        : rootSource.IsChat
            ? $"called by chat with args {rootSource.Arguments}"
            : rootSource.IsRequest
                ? $"called by HTTP ${rootSource.Request.HttpMethod} with body {rootSource.Request.RawBody}"
                : "called by a scheduled task";
    await Bot.ReplyAsync($"The root source is `{rootSource.SkillName}` {reason}.");
}
```

```python
# Python example of examining the root source of a signal chain

if bot.signal_event is not None:
    root_source = bot.signal_event.root_source
    reason = str(root_source.pattern) \
        if root_source.is_pattern_match else "chat" \
        if root_source.is_chat \
        else f"http {root_source.request.http_method} with body {root_source.request.raw_body}" \
        if root_source.is_request else "other"
    bot.reply(f"The root source is `{root_source.skill_name}` {reason}.")
```

```js
// JavaScript example of examining the root source of a signal chain

if (bot.signalEvent) {
    const rootSource = bot.signalEvent.rootSource;
    const reason = rootSource.isPatternMatch ? `called via ${rootSource.pattern}`
        : rootSource.isChat ? `called by chat with args ${rootSource.args}` 
        : rootSource.isRequest ? `called by HTTP ${rootSource.request.httpMethod} with body ${rootSource.request.rawBody}` 
        : 'called by some means not understood by anyone';
    await bot.reply(`The root source is \`${rootSource.SkillName}\` {reason}.`);
}
```

If multiple skills subscribe to the same Signal name, all those skills will be called when that signal is raised. There isn’t a way to order the execution of these Signaled skills, so keep in mind that they may execute in arbitrary order.

## Signal Ideas

Here are some ideas for using Signals in your own workflows:

* Register a `page` listener for the `pager` skill so that other skills can raise PagerDuty events
* Register an `issue` listener for the `github` skill so that other skills can create GitHub issues (perhaps the `404-finder` will create an issue each time it discovers an unexpected 500 error while crawling)
* Register a `high-five` listener for the `sparkle` skill so that Abbot can automatically sparkle people for running a specific skill.

When combined with [Patterns](https://blog.ab.bot/archive/2021/11/03/introducing-patterns/), the possibilities for automating work from chat are endless with Abbot.

Happy shipping!
