---
title: "Introducing Patterns"
description: "Patterns give Abbot the ability to respond to any message, not just commands."
tags: [patterns,abbot]
excerpt_image: https://user-images.githubusercontent.com/19977/139963499-5f0db4c3-a72e-4bad-8419-d1ac247c676c.jpeg
author:
    avatar: https://2.gravatar.com/avatar/cdf546b601bf29a7eb4ca777544d11cd?s=160
    name: haacked
---

Until recently, to call an Abbot skill, you issued a command that specified the skill by name (e.g. `@abbot grafana help` or `.grafana help` for short).

Since every skill has a unique name, this approach is easy to reason about. It's always clear which skill will respond to a given command. However, there are cases where it would be nice for Abbot to not be restricted to direct commands. For example, you may want alternative ways of calling a skill that feel more natural in the situation. Or you may want Abbot to act more proactively to content in messages such as when someone mentions a tracking number.

![Puzzle with last piece fitting in - Image dedicated to the public domain](https://user-images.githubusercontent.com/19977/139963499-5f0db4c3-a72e-4bad-8419-d1ac247c676c.jpeg "Puzzle image dedicated to the public domain by the author.")

Well now such skills are possible with the new Abbot feature, Patterns! Patterns allow you to configure listeners in Abbot that pay attention to specific words and phrases. When a pattern matches, the associated skill is called. Patterns can match the beginning or end of a message, or even use a regular expression to match a message.

Patterns can be useful to your team, because they can connect Abbot skills to everyday conversations that your team has in chat. There are simple use cases like keeping track of karma points or checking if a website is down, or more complex ones like linking to external issues, or even monitoring chat for errant yubikey clicks.

Patterns give Abbot a lot of flexibility: now Abbot can respond to any kind of message in chat, not just ones where it’s been mentioned directly. We’ve created some example skills you can use that consume patterns, or you can see a deeper dive in [this blog post]({% post_url 2021/2021-11-03-how-cascadiajs-uses-abbot %}) about how the Cascadia JS conference used Patterns to power its conference live feed.
