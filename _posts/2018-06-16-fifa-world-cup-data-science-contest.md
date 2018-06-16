---
layout: post
title: 30 days of Data Science, Social Marketing and Writing during the 2018 FIFA World Cup  
categories:
  - data analysis
show_comments: true
tags: FIFA World Cup 2018, data analysis, data science
---

Yesterday I had the idea  to push  myself through a 30 days contest of Data Science during the 2018 FIFA world cup. But it's not just about Data Science it's also about writing the results (and hopefully stories) down and doing some
social marketing to spread the word. I think it will be tough and I already noticing it writing this first lines. Everything I wanted to write is in my brain since yesterday, but it's very hard to write it down.

My goal for the contest:

   - Find, scrape and clean data that I need for analysises from different sources
   - Doing data analysis with Python, R and Tableau
   - Publish interesting findings in blog posts, images and dashboards
   - Develop a predictive machine learning model for something ;-) (no idea yet for what)
   - Develop a daily routine of writing
   - Learn to tell stories with data
   - Spread the word on channels like Twitter, Facebook, LinkedIn and Xing and maybe other sources.

I am very curious what's happening on my journey:

## Day 0 (15/06/2018)

Every data science project needs **D A T A**. 

So I first scraped the squads of the different teams taking part in the world cup from [2018 FIFA World Cup squads](https://en.wikipedia.org/wiki/2018_FIFA_World_Cup_squads) with [Python](http://python.org).

Well, this took some time and at end I noticed that the data is ready to download on [Kaggle](http://kaggle.com):

   - [FIFA WORLD CUP 2018 Players](https://www.kaggle.com/djamshed/fifa-world-cup-2018-players), whch incldes weight, height, birth date, club etc.
   - I want to use [FIFA 18 Updated Dataset for EA computer game](https://www.kaggle.com/piyushgandhi811/fifa-18-updated-dataset/data) which includes different metrics for 17,000 players for the computer game [EA FIFA 18](https://www.easports.com/de/fifa/2018-fifa-world-cup-update).
     This metrics are very accurate as the community and even the players themselves are discussing about them. Based on this discussions these metrics are permanently changed. 
   - Also promising is this: [FIFA 18 vs Real Life Football](https://www.kaggle.com/michaelmallon/fifa18-vs-reallife/data). 


What's always underestimated is the time it takes to clean the data. But I had another problem that took me a while to solve: I wanted to join the data from the FIFA WORLD CUP 2018 Players dataset with the FIFA 18 Updated Dataset for EA computer game, but this
was not that easy as there was no unique criteria to do the join. So I had to do join based on a similarity of the names. In the one data set there is e.g "C. Ronaldo" and on the other one "Christiono Ronaldo". I used the similarity algorithms Levenshtein and Jaro Winkler.
I got pretty good results, but there were about 70 players I had to fix by hand, which took me a while.

## Day 1 (16/06/2018)

Today is Saturday and I have many tasks to do like cutting the lawn, shopping and other stuff and not forget the time with the family. 
So today (and tomorrow) I have to cheat a bit and take a shortcut and just publishing one visualization to reach the goal of publishing somethin daily.


It's time for doing a exploratory data analysis (EDA) on the data. I like to use [R](https://www.r-project.org/) for the task. I don't like the syntax of the language too much compared to [Python](https://www.python.org/), 
but I love Rmarkdown  and R's libraries dplyr (Grammar of data manipulation) and ggplot2 (Grammar of visualization) as they simplify the EDA

So my first question I want to answer from the data:

### Who are the 4 most valuable players in the different teams?

Voilà:


![Most Valuable Players by Team](/assets/img/most-valuable-players_by-team.png)
