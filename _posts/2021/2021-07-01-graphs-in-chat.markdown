---
title: "Visualize Grafana dashboards in chat"
description: "The grafana skill lets you quickly see all your Grafana dashboards and visualize custom graph queries without leaving your chat window."
tags: [grafana,graphs]
excerpt_image: https://user-images.githubusercontent.com/310137/124177982-4369b600-dab1-11eb-84eb-33f3ba1d23c4.png
author:
    avatar: https://gravatar.com/avatar/dc6fa84a4a864f9627e220b716572cba?s=160
    name: shana
---

An image is worth a thousand words, which is why [Abbot](https://ab.bot/) can now help you visualize your graphs in chat with the [grafana](https://ab.bot/packages/aseriousbiz/grafana) skill!

List all your dashboards and quickly query your panels, generating graphs with custom time ranges and template variables to suit your visualization needs.

## Setting it up
Before you can start visualizing all your graphs, you need to tell Abbot where to go to get them.

Start by generating a Grafana API key on your Grafana host, usually found at `https://your-grafana-host/org/apikeys`. Then, visit [https://ab.bot/skills/grafana](https://ab.bot/skills/grafana) and click "Manage skill secrets" to add a secret named ___GrafanaKey___, with the key you created.

In chat, tell Abbot where your Grafana host is with

```bash
@abbot grafana config host https://your-grafana-host
```

Once the API key and the host configuration are done, you're ready to graph!

## Graphs in action
The main command for the Grafana skill is `db`, for dashboard. To see all the panels of your _Home_ dashboard, run

```bash
@abbot grafana db home
```

Dashboard names are case insensitive and can be partial, so if you have a dashboard with a long name, you don't have to write it all in full.

![Screenshot showing several graphs of the Home dashboard](https://user-images.githubusercontent.com/310137/124183379-6a77b600-dab8-11eb-92a7-91af9d60b21c.png)


You probably don't want to see all the panels of a dashboard all the time though - you will likely have specific graphs you want to generate at certain times. To see a graph of a specific panel of a dashboard, add a colon and the panel name after the dashboard name to the `db` query, like so:

```bash
@abbot grafana db home:logins
```

Panel names are case insensitive, but they need to match fully. If they have spaces, you'll need to quote them:

```bash
@abbot grafana db "home:memory / cpu"
```

![Screenshot of a panel with a long name](https://user-images.githubusercontent.com/310137/124185001-a7dd4300-daba-11eb-992d-c92e91ec6fd1.png)

You can also use the panel ID instead, so you don't have to type the whole thing every time! Use the `panels` command to see all available panels of a dashboard and get the ID you need:

```bash
@abbot grafana panels home
@abbot grafana db home:11
```

![Screenshot of panels of a dashboard](https://user-images.githubusercontent.com/310137/124184325-b37c3a00-dab9-11eb-883f-3640c0b3a19a.png)

![Screenshot of a graph of a panel by ID](https://user-images.githubusercontent.com/310137/124185258-05718f80-dabb-11eb-8f26-2edfefcfd3e8.png)


## Customize your graphs
Grafana graphs support templated variables and a host of other customization options, and you can use all of those from chat as well - the skill supports custom queries with time ranges, template variables, timezone specifications and size constraints.

Most often, you'll want to customize the time window of your graph. You can specify a range by adding time parameters to your query:

```bash
@abbot grafana db home:3 2d 1d
```

This generates a graph of the `logins` panel in the `Home` dashboard with a time window starting 2 days ago and ending 1 day ago.

Time parameters are optional, and if you only have one, it will be interpreted as a window of "now - Time" to now.


```bash
@abbot grafana db "grafana dashboard:client side full page load" 24h var-app=backend var-server=backend_01 var-server=backend_03 var-interval=1h
```

The more complex query above generates a templated Grafana graph with a time window of 24 hours, customized with several variables.


![Screenshot showing a graph with custom ranges and template variables](https://user-images.githubusercontent.com/310137/124180188-28e50c00-dab4-11eb-9398-d3333323bae0.png)

Any parameter that Grafana supports in a query can be passed to the skill as `key=value` pairs, like timezone specifications or custom graph sizes:


```bash
@abbot grafana db home:3 tz=Europe/Lisbon width=1000 height=1000
```

![Screenshot showing a graph with a custom timezone and size](https://user-images.githubusercontent.com/310137/124186062-0ce56880-dabc-11eb-860c-27846912bb14.png)

Run `@abbot help grafana` for general help with the skill, or `@abbot grafana help [command]` for detailed help of each skill command.

You can find the source for this skill on our [skills repository](https://github.com/aseriousbiz/abbot-skills/blob/main/skills/grafana.cs). If you have any issues or improvement suggestions, go ahead and open an issue or send us a pull request in that repository.

Happy graphing!

.
