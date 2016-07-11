---

layout: post
title: GENDER MOVIE FUN
excerpt: "Assessing gender equality in movies"
categories: [Metis Projects]
comments: true
image:
  feature: https://cdn.theatlantic.com/assets/media/img/mt/2014/03/heathers1/lead_large.jpg
  credit: 
  creditlink:
---


For our first solo project at the Metis bootcamp, we were told to work with scraping tools, movie data and linear regression.
Inspired by Matt Daniels and Hannah Anderson's[polygraph.cool](http://polygraph.cool/movies) Film Dialogue breakdown, I shaped my second Metis data project around their collected movie data. This data includes the number of lines spoken by male characters
in a collection of 2000 movies.

Matt Daniels and Hannah Anderson had already worked hella hard  and split the movie scripts by character AND by comparing to imbd, assigned a sex and age to each character's actor/actress. The resulting metadata and some of the data is publicly available on Matt Daniels' github page for scrutiny and further data fun. 

I wanted to use all this lovely data to see whether it was possible to predic the era a movie belongs to based on it's character and line ratio. But I thought in order to do this effectively, I should probably include genre and for this I needed to scrape, scrape, scrape.

#### Web scraping with Beautiful Soup

I collected data on genre by scraping Box Office Mojo using [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/). I just collected data on all the movies they have on the site. I actually also scraped info on budget and domestic gross in case I decided to use that later on. 

#### Data wrangling
Now I had all this data from Box Office Mojo and I only needed the genres for the movies from the polygraph movies. So I decided to join the two dataframes I now had using `pandas.merge`.

Merging the two dataframes together was annoying at first because movies will have the same title but come out in different years. Or the titles will be written slightly differently, and sometimes there were random dollar signs and other markings in the title that definitely did not belong there. _Oh the joys of data cleaning_. I wapped the year and title in a new column and merged the dataframes on that. This worked wonders except for one little movie : *Crash* which according to imdb came out in 2004 but in the dataset it came out in 2005. Whoops. Other than that the movies were solidly matched. 

I had to create dummy variables of the genres, which just creates a column for each genre and assigns a 1 if the movie is of that genre and a 0 if it's not. But I had 67 genres so this drastically enlarged my feature space and I didn't want to make life so difficult for myself.

To reduce the feature space, I condensed some of the more obscure genres into broader ones. Obscure meaning 'Comedy' was split into 'Comedy', 'Comedy/Drama', 'Comedy/Thriller', 'Comedy/Crime', etc. It was a little tricky sometimes, for example, does 'War Romance' go into 'War' or 'Romance'? In the end I manually condensed the genres, reducing the 67 genres to 10 using the pandas `replace` function.











