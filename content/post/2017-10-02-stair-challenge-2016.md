---
title: Stair Challenge
author: Kohleth Chia
date: '2017-10-02'
slug: stair-challenge-2016
cover: /post_imgs/stairChallenge_oddEven.png
categories: []
tags: []
---

My employer ran a 4-week stair challenge in October last year (2016). Basically, staffs were encouraged to use the stairs instead of the lift to commute within our 5+ storey building. Clipboards with paper forms were placed at the entrance of the stairways on multiple levels, and we could record/report our stair usage. The most frequent stair user(s) at the conclusion of the campaign wins a prize.

And I participated (by report/recording my stair usage).

Of course, I didn't win (not even close). But I asked for the data and they gave it to me. Statistician / Data Scientist / (people who analyse data) don't often get to become part of their own data (for some good reasons such as unbiased-ness), but from time to time it is actually fun to do so. And this time, using my own 'domain knowledge' I actually manage to find something interesting.

### 1. People don't consistently use stairs
Technically, each intra-building commute consist of a pair of trips: to and from. So, if people have been consistently using stairs, their daily stair usage should be an even number. But guess what, three quarters of daily usages are actually odd numbers!

![Yes you are right I am using a pie chart.](/post_imgs/stairChallenge_oddEven.png)

So here I have an ~~ques~~actionable insight: If you want to encourage people to use stairs, you might as well go with the natural tendency and use slogans like *take at least the stairs down!* or *take the stairs back!* or *Start from 50% stairs!* (terrible slogans I know... but you get the idea)

### 2. Campaign participation drops in the first two weeks
Below is a plot of the number of participants (people who have reported their stair usage) everyday for the entire campaign (the campaign started on a Wednesday). You will see that we started from 55-60 participants in the first week, but lost a third of those in week 2, and half of those by week 3 before it stabilized. You will also notice that Friday seems to have a consistently low count. But I suspect (no proof) that this is simply due to less people working on Fridays.

![](/post_imgs/stairChallenge_Nparticipants.png)

So my ~~ques~~actionable insight is this: If you are running a similar campaign. You should definitely *put out reinforcement message each week.*

### 3. Rain doesn't seem to affect stair usage
This is difficult to test rigorously due to the non-experimental (observational) nature of the data generating mechanism. But during the campaign, I remember hesitating whether to take the lift or not when it was raining. This was because I (or more accurately my umbrella) was dripping water, so I just wanted to get to my desk asap. 

To test this, I paired the usage data with [rainfall record](//www.bom.gov.au/climate/data/). The figure below plots the median personal stair usage across the campaign. The blue points are the top 10 stair users, and the red points are the rest. The size of the point correlates with rainfall of that day. If rain discouraged stair use, we should see larger circles with lower count. But I could not see a consistent effect.

![](/post_imgs/stairChallenge_rainfall.png)


## Remarks
This is a fun analysis to do because I was part of the data generating mechanism. I had intimate knowledge of what to look for, and this increased the fun of finding the answer.
