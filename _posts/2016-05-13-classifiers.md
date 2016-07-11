---

layout: post
title: CLASSIFICATION
excerpt: "30 category classification - better with defined features or brute force?"
categories: [Metis Projects]
comments: true
image: 
  feature: http://www.biologydiscussion.com/wp-content/uploads/2015/10/clip_image004_thumb223.jpg 
  credit: 
  creditlink: 
---


## Before I start...

Perhaps the two biggest personal lessons I learnt at Metis were learnt during this project. 

### Procrastination will kill you.

I did not know what to do. And I did not stick to any idea that I had. Initially we started in groups with vague aims of classifying a genre into a subgenre. We went to the spotify music hackathon, bright-eyed and bushy-tailed and ready for adventure. Though the hackathon itself was fab and the speakers were inspiring - we didn't figure out our subgenre problem. The million song dataset wouldn't fit on my AWS, _and_ the spotify API didn't return the things that we were interested in. The fields were empty. Additionally, the awesome python libraries that we could use to analyse audio files was not cooperating with my python3. All of these barriers would have been circumventable if only I believed in the cause. I said yes to this because I was excited to work with music but I classifying genres into subgenres was not something I felt I could put my heart into. And so all of these hiccups felt all the more monumental. With retrospect I could have done some pretty neat classification problems surrounding music.

and I will probably explore these now I have some 'free' time (ahum. job-hunting.) but at the time these did not come to me. I looked up datasets and debated playing with calcium signalling data ([is a neuron of interest or not?](http://neurofinder.codeneuro.org/)), or are mushrooms edible or not, or even does this person have Parkinson's or not? Some SUPER interesting things kicking about there but procrastination killed them all. And 36 hours before presentation time, I decided to classify leaf images to corresponding tree species. This was fine, but given the time strain I had very little opportunity to do deeper interesting analysis or to seriously consider the problem or uses of this classifier.

Moral of the story: Find a project and _stick to it_. You can make something interesting out of everything. 

### Don't try and change the world with every project. 

You will __always__ be disappointed. __Always__. One of my main issues is having to make everything I do 'important' to justify putting time into it. But ultimately this course is about learning the ropes. Taking on seemingly less serious topics will allow faster blooming of my technical capabilities.
I have always appreciated the little things and I get excited by small logic problems and even things like data cleaning (_HOW??_) but if I have to present a project I have had a tendency to overthink and search for the much much bigger picture. This has made it difficult to get excited about things that just have a simple bigger picture. Or projects that are only focused on 'increasing revenue'. But I have realised how detrimental this is. For the leaf project I went to lengths to make up a story about the APOCALYPSE! Are you ready?

_...it is 50 years in the future, and infrastructure has collapsed. It is more and more difficult to find reliable, safe and affordable medicine and so the people are turning back to nature for their remedies. For this reason they need a classifier to tell them, based on an image of the leaf, what the tree species is and whether that plant is medicinally useful to you..._

Yes. Our fantastic teacher Vinny pointed out that actually we don't need an apocalyptic future to justify a leaf classifier and it can be useful for education, park rangers and many other entirely relevant things.

Anyway, these two valuable lessons are now learnt and I shall carry them with me forever.

## The project

I got this dataset from [UCI]() along with a paper by --- explaining his logic and process. I used openCV to extract some of the features he mentioned in his thesis. All of which were present in the dataset itself, but just to get a feel for the openCV package and learn a bit about image processing.

#### basic cleaning

I remember when I started working with and how long it took to get the basics down because everyone assumes they are a no-brainer. So I'm going to mention a little about cleaning the data in every project I post. 

This data was pretty clean already so it was just a case of organising and understanding it.

I replaced string nans with float `nan` so that they would be recognised as non-values specifically.

I grouped my data by species to have a look at the summary of differed variables per species. To see whether something was largely nans and just to understand the metrics a little better because they consisted of things like 'Eccentricity', 'Solidity' and 'Isoperimetric Factor'... (see end for explanation of these terms).

I created a reduced dataset that didn't have things in it I wasn't planning on using in my analysis and I `pickled` that so that I could just load it up again if I quit the programme.

__The test-train split__. I had 30 species and only 8-15 specimens per species so it was important to do a stratified split so that all the species were present in both the training and the test set.

I used `seaborn` and `matplotlib` to plot some of the features(variables) in the training set.
Included having some fun with violin and swarm plots. Just another way of visualising the distribution within a feature. This violin plot is the coloured section, which represents the density of data points at a certain value and the swarm plot just puts the actual data points on there as well.

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/class_swarmplot.png?raw=true)

Using feature importance analysis to pull out the most important features can be quite prudent. Sometimes some of the features are mathematically related to each other. Also, in the leaf dataset there were 16. Lots of variables means lots of dimensions which makes it harder to glean relevant insights. 

It couldn't hurt to prune some out. 

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/class_features.png?raw=true)

I tried running my classifier models with the top 9 (and then 10, and then 11) features but my accuracy score for each model dropped a lot, some by 20 points. So I stuck with all 16.

A little sidenote - I plotted a scatter matrix for my features which is just a matrix of scatter plots that shows you how each pair of variables are related. Down the diagonal (where the one variable would be compared with itself) is a KDE plot for that particular variable. That just shows the distribution within the variable. This is a great and reasonable quick way to visually understand your data and the relaitonships within it - that is, if you don't have more than 10 or 15 variables..

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/ClassScatter_matrix.png?raw=true)

### A little summary of the classifiers

##### LOGISTIC REGRESSION

__Logistic regression is used to predict the odds of being a case based on the values of the independent variables (predictors)__

Logistic regression is just regression where the dependent variable (the thing you are predicting) is categorical (splits into categories). This is most effectively used, or designed for, binary models. Usually in cases of multple categories (that are unordered) the regression model to use is _Multinomial Linear Regression_ which doesn't have a handy function in scikit learn (sit tight for more on this). When applied to a problem with multiple categories, like the leaf dataset, an logistic regression analysis will assess the probability that something is in one category compared to all other categories. 

Logistic regression assumes a clear cut off [images]

##### SVM

SVM stands for __Support Vector Machine__. It's basic idea is that you define a decision boundary using the 'widest street' approach. This means that you want to draw a line that has on either side of it, the biggest margin possible between the two categories. Wihout a __kernel__, SVMs are only really useful for data that is linearly divided. It looks a little like this:

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/svmsimple.jpg?raw=true)

Using a kernel means drawing the line in a higher dimensional space and then reducing the dimensionality so that the line no longer looks straight. Simple, right? When you use kernels correctly SVMs can be super useful for all kinds of classification problems. But as always you need to be careful of overfitting. 

I find that this [MIT lecture](https://www.youtube.com/watch?v=_PwhiWxHK8o) is super helpful. I also love chalkboards.

##### K NEAREST NEIGHBOURS

I also attempted to classify the leaves using a KNN algorithm.

##### DECISION TREES and RANDOM FORESTS

Decision trees and random forests are probably my favourite type of classification algorithm because they are really intuitive  but also really effective in lots of situations. You run the risk of overfitting, but random forests help there. 

A decision tree consists of a starting point, where you have ALL your data, and then you split that data by asking a simple yes/no question and splitting the data accordingly. For example, is your leaf longer than 10cm? Yes: contains all leaves longer than 10cm, and No: contains all leaves shorted than 10cm. You set how many questions you want to ask, or in the correct lingo, how many levels you want the tree to contain and _Voilá_! You end up with categories into which new leaves will be classified. With labeled data, the accuracy score is the probability that a leaf is correctly classified.

Random Forests just do the same process multiple times (lots of decision trees) and then take the mode or mean prediction of the sum of the trees. The good thing about Random Forests is that they can reduce the risk of overfitting that can happen with Decision Trees. 

And LOW AND BEHOLD, for my leaf dataset the Random Forest classifier was the most accurate. Which was just perfect. Because leaves. And trees. And forests. Geddit?

Here's the breakdown:

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/class_accuracy.png?raw=true)

And the classification report, which is just a function in sklearn:

--CLASSIFICATION REPORT--

### Brute forcing it

I wanted to see whether using a naive bayes image classifier would return better or worse results than the above, more restrained process of picking out specific mathematical representations of the image.

So I loaded all the actual images of the leaves, which were luckily attached to the dataset, and binarised them using the cv2 package. This returns a vector where each value represents the likelihood that that pixel will be black. 










