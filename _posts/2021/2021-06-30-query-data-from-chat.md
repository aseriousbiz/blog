---
title: "Query databases from chat with Abbot"
description: "Chat is the perfect place to review data with your team. Abbot makes it easy to query your data and share it in chat."
tags: [sql,data,dataclip]
excerpt_image: https://user-images.githubusercontent.com/279389/124063486-d4745a80-d9e7-11eb-913a-e44ee5b46c55.png
author:
    avatar: https://avatars.githubusercontent.com/u/279389?v=4
    name: pmn
---

Chat is a great place to review data together. Abbot has always made interacting with APIs easy; and now Abbot can also talk to MySQL and Postgres databases.

Here are some examples of what you can do with this new power:

## Run predefined reports from chat

It can be a hassle to get reports from your database and into a spreadsheet, just so you can email them to your team. Having a set of pre-defined reports that your team can run at any time gives them the ability to review data on demand.

Let's build a sql reporting skill in Abbot using Python. We'll use Pandas, since it provides so many tools for data manipulation. We will also include SQLAlchemy to manage the database connection. You will need a connection string to a database that Abbot can reach in order to write this skill. C# and JavaScript skills can also connect to databases! 

Start a new Python skill called `reports`, then create a secret called `connstr` with the connection string you created for your database. Once the skill and secret are created, add these lines to your skill:

```python
import pandas
import sqlalchemy

connstr = bot.secrets.read("connstr")

# Create the database connection
engine = sqlalchemy.create_engine(connstr)

# The query to run
query = "select 1;"

# pandas.read_sql returns a dataframe
df = pandas.read_sql(query, engine)

# Dataframes can return markdown using `to_markdown()`. 
# Wrapping the table with three backticks will render the results as code.
results = "```{}```".format(df.to_markdown())
bot.reply(results)
```

These few lines of code will form the basis of everything we we'll do through this post. Running the skill now should return a result that says "1". 

Let's experiment and make some predefined reports for people to run. Since your database will have its own unique schema, you will need to write some simple queries to work with. It is a good idea to limit all your queries to only a few results so that you don't flood chat with too much data.

```python
# Create the database connection
engine = sqlalchemy.create_engine(connstr)

queries = {
    "newusers": """ SELECT "Username", "CreatedAt" 
                    FROM "Users" 
                    ORDER BY "CreatedAt" 
                    DESC LIMIT 10;""",
    "usercount": """ SELECT DATE(date_joined) AS DAY, COUNT(id) AS NewUsers 
                     FROM users 
                     GROUP BY DATE(date_joined);"""
}

query_keys = queries.keys()

# Check to see if the user has asked for a predefined query.
if bot.arguments in query_keys:
  data = pandas.read_sql(query_keys[bot.arguments], engine)
  result = "```{}```".format(data.to_markdown())
else:
  result = "You didn't pick an available query."

bot.reply(result)  
```

Now when users say `@abbot reports newusers` it will run the query found in `newusers`. There isn't much in the way of error handling in this example, but you can see how simple it is to fill out queries for people to run. We published a [sql skill](https://ab.bot/packages/aseriousbiz/sql) in the Package Directory that you can use as the basis for your own querying from chat.

## Searching for information
Customer-facing teams often need to look up user data when talking with customers. Instead of building an entire website for customer lookup, it can be replaced with a simple Abbot skill. 

Create a new skill called `lookup`, and copy over the same template we used from the `reports` skill. We will make a simple change to the query, and everything should work. Note that we parameterize the query to spoil the fun of any trolls we might have in our chat.

```python
# The query to run
query = """
        SELECT "Id", "Name" 
        FROM "Organizations" 
        WHERE "Name" ILIKE %(orgname)s
        """

df = pandas.read_sql(query, 
                     engine, 
                     params={"orgname": "%{}%".format(bot.arguments)})
```

Run this skill by saying `@abbot lookup yadda` where `yadda` is contained in the Name field of your table. This will give you a list of organizations that match. Get extra credit by forming the correct URLs for each organization when you return the data so that your customer teams can click a link to view their customer record.

## The Danger Zone
With great power comes great responsibility. It's possible to create a skill that allows people to execute arbitrary SQL commands from chat. It's up to you and your team to decide if that's a good idea or not; if you do decide to walk on the wild side, Abbot has built-in auditing and [access controls](https://youtu.be/6NHMyyWZtrU). Be sure to limit who can run these sorts of skills in your chat by restricting the skill before you release it.

## What about Heroku Dataclips?
If you're using Heroku Dataclips, fetching the data and returning it in chat can be as simple as the following (make sure `clipurl` contains the URL for your Dataclip CSV): 

```python
import pandas
df = pandas.read_csv(bot.secrets.read("clipurl"))
bot.reply("```{}```".format(df.to_markdown()))
```

That's all there is to it! 

## Wrapping up
It's easy to imagine what's possible once an Abbot skill can connect to external databases, especially when combined with [HTTP Triggers](https://docs.ab.bot/guides/triggers/), access controls, and built-in auditing. The source code to the `sql` package is [available on GitHub](https://github.com/aseriousbiz/abbot-skills/blob/main/skills/sql.py) as well, if you'd like to use it for the basis for your own skills.

Please let us know if you have a use case that requires you to connect with a database we don't yet support. We'd love to talk to you about it! 

