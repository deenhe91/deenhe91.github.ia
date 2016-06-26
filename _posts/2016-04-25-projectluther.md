---

layout: post
title: Linear regressions and movies
excerpt: "Assessing gender equality in movies"
categories: [Metis Projects]
comments: true
image:
  feature: https://cdn.theatlantic.com/assets/media/img/mt/2014/03/heathers1/lead_large.jpg
  credit: 
  creditlink:
---


For our first solo project at the Metis bootcamp, we were told to work with scraping tools, movie data and linear regressions.
Inspired by Matt Daniels'[polygraph.cool] (http://polygraph.cool/movies) Film Dialogue breakdown, I shaped my second Metis
Data project around his collected movie data. This data includes the number of lines spoken by male characters
in a collection of 2000 movies - every movie for which he could find a script - as well as the main character breakdown within each of those movies, whether they were male or female and how many lines they spoke.

As this project was built around linear regression, I collected data on budget, genre and domestic gross by scraping Box Office Mojo using Beautiful Soup and selected features to build a model to predict the ratio of female to male lines in a movie. Matt Daniels and Hannah Rogers had already worked hella hard and collected thousands of movie scripts and split those scripts by character AND by comparing to imbd, assigned a sex and age to each character's actor/actress. The resulting metadata and some of the data is in the data folder in my [github] (https://github.com/deenhe91/project-movie/tree/master/data_files) because they made it all publicly available for scrutiny and further data fun. 


#### ALERT
Joining the two dataframes together was annoying at first because movies will have the same title but come out in different years. Or the titles will be written slightly differently, OR there were just random dollar signs and other markings in the title that definitely did not belong there. _Oh the joys of data cleaning_. I didn't use fuzzy matching here, as I saw many of my classmates use, but I _did_ wap the year and title in a new column and joined the dataframes on that. This worked wonders except for one little movie : *Crash* which according to imdb came out in 2004 but in the dataset it came out in 2005. Whoops. Other than that the movies were pretty solidly matched. 

Pandas to the rescue **SO MANY TIMES**. I was introduced to statistical coding using R, so python before pandas was a nightmare. 

Let's see, grouping by genre was interesting. For example, does 'War Romance' go into 'War' or 'Romance'? This had to be done by hand but perhaps I could cluster movies together based on attributes and see if they still match those relatively arbitrary genre groupings.

While many others were focused on predicting monetary success of a movie, or figuring out the relationship between budget and gross (all fairly interesting), I just wanted to do some descriptive analysis and see whether I could predict the year a movie came out based on it's character and line ratio.
