---
title: "Introducing Patterns"
description: "Patterns give Abbot the ability to respond to any message, not just commands."
tags: [patterns,abbot]
excerpt_image: 
author:
    avatar: https://2.gravatar.com/avatar/cdf546b601bf29a7eb4ca777544d11cd?s=160
    name: haacked
---

Until recently, Abbot skills could only be called by calling them directly by name (i.e. “@abbot grafana help”). This makes Abbot easy to get started with and reason about. However, there are entire categories of problems that Abbot can help out with where it doesn’t make sense to call a command. 

Since Abbot can respond to ambient information, we’re excited to announce a brand new feature called Patterns. Patterns allow you to configure listeners in Abbot that pay attention to specific words and phrases. Patterns can be configured to look at the beginning or end of a sentence, or even a regular expression. 

We think that Patterns will be useful to your team, because they can connect Abbot skills to everyday conversations that your team has in chat. There are simple use cases like keeping track of karma points or checking if a website is down, or more complex ones like linking to external issues, or even monitoring chat for errant yubikey clicks. 

Patterns give Abbot a lot of flexibility: now Abbot can respond to any kind of message in chat, not just ones where it’s been mentioned directly. We’ve created some example skills you can use that consume patterns, or you can see a deeper dive in [this blog post]({% post_url 2021/2021-11-03-how-cascadiajs-uses-abbot %}) about how the Cascadia JS conference used Patterns to power its conference live feed.



&nbsp; 
