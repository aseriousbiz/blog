---
title: "Support User Interactions With Buttons"
description: "Abbot skills can now support user interactions through user interface elements. In the latest release, Abbot skills may include buttons in a response that the user can click (Teams and Slack only)."
tags: [abbot, ui, buttons]
excerpt_image: 
author:
    avatar: https://2.gravatar.com/avatar/cdf546b601bf29a7eb4ca777544d11cd?s=160
    name: haacked
---

Abbot skills get a lot done despite being text based. But sometimes a human just wants to click a button. With the latest release of Abbot (`0.15.0-beta`), Abbot supports user interface elements such as buttons. This enables skill authors to create approval workflows, simple games, etc. within chat.

Be sure to read the [documentation for the feature](https://docs.ab.bot/guides/ui-elements/?tabs=tabid-cs).

In this blog post, we build on the documentation to build a fun little Bot or Not skill. The skill is a game where the user is presented with an image and clicks a button to indicate whether the image is a bot or not.

## The pieces

There are a few pieces to building such a skill. The first is having a list of candidate images. In this case, the list is hard-coded. But it wouldn't be hard to support adding candidate images and storing them in Abbot's brain (`Bot.Brain` in C# and `bot.brain` in JavaScript and Python).

There's two branches the skill needs to handle. The first is when the skill is first called. In that branch, the skill calls the `Bot.ReplyWithButtons` (or `bot.replyWithButtons` for JavaScript and `bot.reply_with_buttons` for Python) method to present a hero card with a random candidate image and two buttons.

The second branch handles the button click. For that, the skill checks if `Bot.IsInteraction` (or `bot.isInteraction` for JavaScript or `bot.is_interaction` for Python) is true. It's `true` if the skill is being called from a button click. In the future, we plan to add support for more types of UI interactions, hence the name is `IsInteraction` and not `IsButtonClick`.

When a button is clicked, the skill is called with the Button's value as the arguments to the skill. Here's the arguments set up in Python.

```python
    yes_answer = "yes " + ("correct" if is_bot else "wrong")
    no_answer = "no " + ("correct" if not is_bot else "wrong")
```

The value of each button passes two arguments back to the skill. The first is the answer that the user selected (either "yes" or "no"). The second argument is whether that answer is "correct" or "wrong". The button value is never presented to the user, so we're not revealing anything here.

When the user clicks on a button, those two arguments are passed back to the skill (along with `IsInteraction=true`) and the skill reports the result and presents a new candidate.

Note that there's room for a lot of improvements left as an exercise to the reader. Here's a few that come to mind:

1. Store the candidates in the brain.
2. Do not show the same candidate twice. The game ends when all candidates have been shown.
3. Tracking who clicked the button so a user may only click a button once.

I didn't do these because it would make the sample more complicated than it needs to be. After all, I implemented it three times!

## Skill Code Listings

The following shows the code for the skill implemented in each of the languages that Abbot supports: C#, JavaScript, and Python.

### Bot or Not C#

```csharp
var candidates = GetCandidates();

if (!Bot.IsInteraction) {
    await AskBotOrNotAsync(candidates);
}
else {
    var (answer, correct) = Bot.Arguments;
    await Bot.ReplyAsync($"{Bot.From} answered {answer} which is... {correct}!\n\nLet's do it again!");
    await AskBotOrNotAsync(candidates);
}

IReadOnlyList<Candidate> GetCandidates() {
    // These could be stored and retrieved from Bot.Brain, but I leave that as an exercise for the reader.
    return new Candidate[] {
        new("R2D2", "https://user-images.githubusercontent.com/19977/119183463-fc57c200-ba28-11eb-9de8-9d83d77af310.png", true),
        new("Wall-E", "https://user-images.githubusercontent.com/19977/119184170-e8609000-ba29-11eb-9eea-e458add34f1f.png", true),
        new("Rosey", "https://user-images.githubusercontent.com/19977/119184464-41c8bf00-ba2a-11eb-959a-dd2821796084.png", true),
        new("Will Smith", "https://user-images.githubusercontent.com/19977/119184726-9cfab180-ba2a-11eb-9c04-7eeca7ea69c0.png", false),
        new("Tree", "https://user-images.githubusercontent.com/19977/119185238-66716680-ba2b-11eb-93a7-33e452961ffd.png", false),
        new("Princess Leia", "https://user-images.githubusercontent.com/19977/119184920-db906c00-ba2a-11eb-8523-109fe87556be.png", false),
        new("Robby the Robot", "https://user-images.githubusercontent.com/19977/119185116-375af500-ba2b-11eb-8e5b-f46ff71b93b4.png", true)
    };
}

async Task AskBotOrNotAsync(IReadOnlyList<Candidate> candidates) {
    var randomCandidate = Bot.Utilities.GetRandomElement(candidates);
    var isBot = randomCandidate.IsBot;
    
    var yesAnswer = "yes " + (isBot ? "correct" : "wrong");
    var noAnswer = "no " + (!isBot ? "correct" : "wrong");
    
    await Bot.ReplyWithButtonsAsync(
        "What do you think?",
        new Button[] {
            new("Yes, that's a bot", yesAnswer),
            new("No, not a bot.", noAnswer)
        },
        "Bot or not?",
        new Uri(randomCandidate.Url),
        randomCandidate.Title,
        "#800080");
}

public class Candidate {
    public Candidate(string title, string url, bool isBot) {
        Title = title;
        Url = url;
        IsBot = isBot;
    }
    public string Title { get; set; }
    public string Url { get; set; }
    public bool IsBot { get;set; }
}
```

### Bot or Not JavaScript

```js
module.exports.BotOrNotJs = (async () => { // We modularize your code in order to run it.

  const _ = require('lodash');
  var candidates = getCandidates();

  if (!bot.isInteraction) {
      await askBotOrNot(candidates);
  }
  else {
      var answer = bot.tokenizedArguments[0].value;
      var correct = bot.tokenizedArguments[1].value;
    
      await bot.reply(`${bot.from} answered ${answer} which is... ${correct}! Let’s do it again!`);
      await askBotOrNot(candidates);
  }

  function getCandidates() {
    // These could be stored and retrieved from Bot.Brain, but I leave that as an exercise for the reader.
    return [
      {title: "R2D2", url: "https://user-images.githubusercontent.com/19977/119183463-fc57c200-ba28-11eb-9de8-9d83d77af310.png", isBot: true},
      {title: "Wall-E", url: "https://user-images.githubusercontent.com/19977/119184170-e8609000-ba29-11eb-9eea-e458add34f1f.png", isBot: true},
      {title: "Rosey", url: "https://user-images.githubusercontent.com/19977/119184464-41c8bf00-ba2a-11eb-959a-dd2821796084.png", isBot: true},
      {title: "Will Smith", url: "https://user-images.githubusercontent.com/19977/119184726-9cfab180-ba2a-11eb-9c04-7eeca7ea69c0.png", isBot: false},
      {title: "Tree", url: "https://user-images.githubusercontent.com/19977/119185238-66716680-ba2b-11eb-93a7-33e452961ffd.png", isBot: false},
      {title: "Princess Leia", url: "https://user-images.githubusercontent.com/19977/119184920-db906c00-ba2a-11eb-8523-109fe87556be.png", isBot: false},
      {title: "Robby the Robot", url: "https://user-images.githubusercontent.com/19977/119185116-375af500-ba2b-11eb-8e5b-f46ff71b93b4.png", isBot: true}
    ];
  }

  async function askBotOrNot(candidates) {
      const randomCandidate = _.sample(candidates);
      const isBot = randomCandidate.isBot;

      const yesAnswer = "yes " + (isBot ? "correct" : "wrong");
      const noAnswer = "no " + (!isBot ? "correct" : "wrong");

      await bot.replyWithButtons(
        "What do you think?",
        [new Button("Yes, that's a bot", yesAnswer), new Button("No, not a bot.", noAnswer)],
        "Bot or not?",
        randomCandidate.url,
        randomCandidate.title,
        "#800080");
    
  }
})(); // Thanks for building with Abbot!
```

### Bot or not Python

```py
import random

class Candidate(object):
    def __init__(self, title, url, is_bot):
        self.title = title
        self.url = url
        self.is_bot = is_bot

def get_candidates():
  # These could be stored and retrieved from bot.brain, but I leave that as an exercise for the reader.
  return [
    Candidate("R2D2", "https://user-images.githubusercontent.com/19977/119183463-fc57c200-ba28-11eb-9de8-9d83d77af310.png", True),
    Candidate("Wall-E", "https://user-images.githubusercontent.com/19977/119184170-e8609000-ba29-11eb-9eea-e458add34f1f.png", True),
    Candidate("Rosey", "https://user-images.githubusercontent.com/19977/119184464-41c8bf00-ba2a-11eb-959a-dd2821796084.png", True),
    Candidate("Will Smith", "https://user-images.githubusercontent.com/19977/119184726-9cfab180-ba2a-11eb-9c04-7eeca7ea69c0.png", False),
    Candidate("Tree", "https://user-images.githubusercontent.com/19977/119185238-66716680-ba2b-11eb-93a7-33e452961ffd.png", False),
    Candidate("Princess Leia", "https://user-images.githubusercontent.com/19977/119184920-db906c00-ba2a-11eb-8523-109fe87556be.png", False),
    Candidate("Robby the Robot", "https://user-images.githubusercontent.com/19977/119185116-375af500-ba2b-11eb-8e5b-f46ff71b93b4.png", True)
  ]

def ask_bot_or_not(candidates):
    random_candidate = random.choice(candidates)
    is_bot = random_candidate.is_bot;

    yes_answer = "yes " + ("correct" if is_bot else "wrong")
    no_answer = "no " + ("correct" if not is_bot else "wrong")

    bot.reply_with_buttons("What do you think?",
      [Button("Yes, that's a bot", yes_answer), Button("No, not a bot.", no_answer)],
      "Bot or not?",
      random_candidate.url,
      random_candidate.title,
      "#800080")

candidates = get_candidates();

if not bot.is_interaction:
    ask_bot_or_not(candidates)
else:
    answer = bot.tokenized_arguments[0]
    correct = bot.tokenized_arguments[1]

    bot.reply('{} answered {} which is... {}! Let’s do it again!'.format(bot.from_user.get('Name'), answer, correct))
    ask_bot_or_not(candidates)
```

## Conclusion

This sample is a fun toy, but it demonstrates the power of the feature to figure in more important use cases. Combine this with [Triggers](https://docs.ab.bot/guides/triggers/) and you could easily build out a workflow approval system initiated by external systems.
