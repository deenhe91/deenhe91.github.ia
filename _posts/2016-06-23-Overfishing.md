---
layout: post
title: OVERFISHING
excerpt: "Keystone project: Building an overfishing app and raising awareness."
categories: [Metis Projects]
comments: true
image: 
  feature: http://www.greenpeace.org/international/Global/international/photos/oceans/2014/GP04BW5.jpg 
  credit: 
  creditlink: 
---
### The final stretch

here's a link to a pdf of the final presentation I gave on career day, 23rd June: [link.](https://github.com/deenhe91/fish_app/blob/master/fish_.pdf)

Overfishing is one of the biggest and most neglected issues of our time. The oceans play a crucial role in maintainig a stable atmosphere and also provide food for billions of people on earth. Maintaining a healthy ecosystem is important everywhere, but in the oceans, an unbalanced and collapsing ecosystem results in substantial effects on the levels of CO2 in our atmosphere and thus has catastrophic effects, not just for economies that rely on fishing to sustain them, but also for fish populations and ultimately our environment.

For our final project at Metis, I clustered fish species into levels of threat, and used these in an [app](github.com/deenhe91/fish_app) so that users can input the name of a fish and find out how threatened or overfished that species currently is.

I got my data from [NOAA](http://noaa.gov), which provided information on over 500 species of fish over an average of 60 years.

I also collected data from the [WCPFC](https://www.wcpfc.int) and did some analysis on that which I'll talk about later.


### Exploratory Analysis

Amidst the many groupbys and pyplots a very clear pattern was demonstrated. Most of these fish were declining. Severely. It was actually quite shocking to view such an obvious trend. And with a bit of digging on the interweb - gotta love it - I found countless organisations, listed at the bottom of this post, who are feverishly campiagning and recruiting to tackle these problems. 


Ian London's [Pandas tricks and tips](https://github.com/IanLondon/pandas_tricks) have been a lifesaver for those pernickety data manipulation tasks in Python. 
So

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/ALBmeancatchrate.png?raw=true)

#### By month, By region, By fish? Fuzzy-Matching

I used the fuzzy matching python package `fuzzywuzzy`, to pull organise fish by just to play around with the package. I used regex on it's own as well, to see whether there was any difference in functionality there. 

First I used fuzzywuzzy to detect all fish in Pacific and Atlantic regions and then `numpy.where()` to pull out indices of all the fish in those regions.

I made separate dataframes for Pacific and Atlantic fish to analyse the differences in overfishing between the two major oceans. 


#### Missing Data

This is just one snapshot of the data I had, namely the mean mortality rate (over the 60 year time period) of fish species in the Pacific...

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/missingdata.png?raw=true "Pacific")

...and Atlantic oceans.

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/missingdata2.png?raw=true "Atlantic")

You can't glean an awful lot from these plots, but it shows you just how much data is missing. Some fish had information on total catch, others had information on available biomass or mortality rate. Some had no information at all. 

I tried using `PyBrain` to fill in these gaps with a recursive neural network.


#### Apping

This part required some serious readjustment of my understanding of the internet, and it was a really enjoyable and enlightening task. I'll do a post on understanding app buidling in the next couple of weeks with a link to the live web app. In the meantime you can have a gander at the app format on my [github page](https://github.com/deenhe91/fish_app).

I built a fish-specific search tool that would allow people to find out how overfished a fish population is. The idea is also to provide alternatives and more of a dashboard breakdown which I am working on right now.

#### Further info and efforts

The Seawatch app, this is very similar to what I have done, but with more time, money and manpower. However, they don't show the changes in the fish and this is a pretty useful chunk of information for the public to digest. It is far more impactful if someone says, "don't eat this fish, look here's why", than just, "don't eat this fish." In the same way you want to educate your children on _why_ they can't go around hitting things, or eat an entire bowl of haribo. That reaction to evidence stays an important influencer of our behaviour and in this way, data visualisations can be completely necessary.

Videos

People

Google
