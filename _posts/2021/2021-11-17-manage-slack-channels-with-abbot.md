---
title: "Manage Slack channels with Abbot"
description: "Abbot skill authors can now manage channels in Slack where Abbot lives. Abbot provides an API to create new channels, invite users to a channel, set a topic or purpose, and archive a channel."
tags: [abbot,channels,slack]
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

Give it a try and let us [know what you think](https://github.com/aseriousbiz/abbot-skills/issues)!