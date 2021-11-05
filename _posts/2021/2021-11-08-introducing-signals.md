---
title: "Introducing Signals"
description: "Signals make it possible for Abbot skills to trigger other Abbot skills."
tags: [signals,abbot]
excerpt_image: 
author:
    avatar: https://2.gravatar.com/avatar/cdf546b601bf29a7eb4ca777544d11cd?s=160
    name: haacked
---

Abbot skills are encapsulated commands that can be run in chat. This works great for single-purpose commands that you might want to build or run, but sometimes skills can get a little large as they try to do more things. 

Today, we are introducing a new feature called Signals. Signals allow skills to effectively trigger other skills. The mechanism is straightforward for skill authors: use `bot.signal` in your skill code along with a signal name (which can be any word), and the arguments to include in the signal call. 

Signals make it possible to automate complex workflows with Abbot. Some use cases include building incident management workflows, decoupling issue management from skills, or even managing on-call rotations.

Signals do not call other skills directly. Instead, skills can register listeners for specific Signals. For example, a skill raises a Signal named “pong”, with an argument of “foo bar baz”. When this skill runs and the Signal is raised, nothing will occur unless a listener is registered. This allows skills to raise Signals cheaply, and forget about what happens after the Signal is raised. <<Add a note about signal chains / cycle detection so people know they can’t call back and forth with Signals>>

Skills register listeners based on Signal names. Any skill can register a listener for any Signal name. If multiple listeners are registered for a Signal name, all those listeners will be triggered. There isn’t a way to order the execution of these Signalled skills, so keep in mind that they may execute in arbitrary order.

The arguments sent in with a Signal are put into the bot’s arguments variable, but skill authors can also check to see if the skill is being run as a result of handling a Signal by looking at “is_signal” in the bot object. 

Let’s build a simple example together. 

Create two skills, one called signal-raiser and the other called signal-receiver. Put this code in signal-raiser: 

<< some code goes here >> 

Now put this code in signal-receiver: 

<< some code goes here >>

Before we go further, run `signal-raiser` and note that `signal-receiver` is not called. That’s because the listener has not been registered yet. Let’s do that now. << insert directions for registering a listener >>. 

Now try running `signal-raiser` again. `signal-receiver` got called! You can create a third skill and register it using the same Signal name to experiment with multiple skill calls. 

Here are some ideas for using Signals in your own workflows: 
Register a “page” listener for the “pager” skill so that other skills can raise PagerDuty events
Register an “issue” listener for the “github” skill so that other skills can create GitHub issues (perhaps the 404-finder will create an issue each time it discovers an unexpected 500 error while crawling)
Register a “high-five” listener for the “sparkle” skill so that Abbot can automatically sparkle people for running a specific skill. 

When combined with Patterns, the possibilities for automating work from chat are now endless with Abbot.

Happy shipping!

