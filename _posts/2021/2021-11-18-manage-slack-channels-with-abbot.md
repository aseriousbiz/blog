---
title: "Manage Slack channels with Abbot"
description: "Abbot skill authors can now manage channels in Slack where Abbot lives. Abbot provides an API to create new channels, invite users to a channel, set a topic or purpose, and archive a channel."
tags: [abbot,channels,slack]
excerpt_image:
    url: https://user-images.githubusercontent.com/19977/142474956-7083a599-d99f-473c-b467-05b5b488e9d4.png
    title: The transcript shown is an example of what's possible.
    alt: Chat session where an incident skill creates a GitHub issue, pages oncall user, and creates a channel. Afterwards, the room is archived
author:
    avatar: https://2.gravatar.com/avatar/cdf546b601bf29a7eb4ca777544d11cd?s=160
    name: haacked
---

Abbot skill authors now have access to the Rooms API (via `bot.rooms`) to manage channels in Slack. This API lets skill authors create new channels, invite users to a channel, set a topic or purpose for a channel, and archive a channel. This feature can come in handy when building out your own incident workflow, especially when combined with [signals](/archive/2021/11/10/introducing-signals/)!

This feature requires that you grant a couple of additional Slack scopes to Abbot, `channels:manage` and `groups:write`. When you log in to [Abbot](https://ab.bot/) with your Slack account, you'll be prompted to grant these scopes in the header:

![Image with header that says "Abbot has new capabilities! These capabilities require updated permissions from Slack. Please reinstall Abbot for the best experience."](https://user-images.githubusercontent.com/19977/142256140-13e09b8d-c475-4baa-ace5-96d490caf426.png "Reinstallation is quick and painless")

To help with learning how this feature works, we published a set of demonstration skills in each language:

* [`room-cs` - C#](https://github.com/aseriousbiz/abbot-skills/blob/main/room-cs/main.csx)
* [`room-py` - Python](https://github.com/aseriousbiz/abbot-skills/blob/main/room-py/main.py)
* [`room-js` - JavaScript](https://github.com/aseriousbiz/abbot-skills/blob/main/room-js/main.js)

The usage pattern for each of these skills is the same:

* `@abbot room-py create {room-name}` - Creates a room with the given name.
* `@abbot room-py topic #room {topic}` - Sets a topic for the specified room
* `@abbot room-py purpose #room {purpose}` - Sets a purpose for the specified room
* `@abbot room-py topic {topic}` - Sets a topic for the current room
* `@abbot room-py purpose {purpose}` - Sets a purpose for the current room
* `@abbot room-py archive #room` - Archives the specified room
* `@abbot room-py invite #room @mention1 @mention2 ... @mentionN` - Invites the specified users to the specified room.

Note that these samples are meant to demonstrate how to use the API, but they're only marginally useful as-is. Where the real power comes in is combining the Rooms APIs with [signals](https://blog.ab.bot/archive/2021/11/10/introducing-signals/) and [patterns](https://blog.ab.bot/archive/2021/11/03/introducing-patterns/) and other skills.

For example, you could have an incident skill that raises a signal when a new Slack channel needs to be created for managing that incident. Another skill could subscribe to that skill and use the Rooms API to create a Slack channel in response.

![Chat session where an incident skill creates a GitHub issue, pages oncall user, and creates a channel. Afterwards, the room is archived](https://user-images.githubusercontent.com/19977/142474956-7083a599-d99f-473c-b467-05b5b488e9d4.png "The transcript shown is an example of what's possible.")

For fun, another thing you could do is have a pattern that changes the room topic in response to certain words being mentioned in chat.

Give it a try and let us [know what you think](https://github.com/aseriousbiz/abbot-skills/issues)!
