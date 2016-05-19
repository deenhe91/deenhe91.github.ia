---

layout: post
title: Linear regressions and movies
excerpt: "Assessing gender equality in movies"
categories: [hello world]
comments: true
image:
  feature: http://jorindevoigt.com/blog/wp-content/wp-content/uploads/artist-talk.jpg
  credit: 
  creditlink: http://www.albilegeant.com
---


Inspired by Matt Daniels' polygraph.cool (HYPERLINK THIS) Film Dialogue breakdown, I shaped my second Metis
Data project around his collected movie data. This data includes the number of lines spoken by male characters
in a collection of 2000 movies - every movie for which he could find a script - as well as the main character breakdown within each of those movies, whether they were male or female and how many lines they spoke. More 
on this can be found here. 

As this project was built around linear regression, I collected features like budget, genre and domestic gross by scraping Box Office Mojo and selected appropriate features to build a model to predict the ratio of
female to male lines in a movie. 






The required features were year of release, genre and character ratio. 