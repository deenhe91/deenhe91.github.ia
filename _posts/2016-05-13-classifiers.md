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
I did not know what to do. And I did not stick to any idea that I had. Initially we started in groups with vague aims of classifying a genre into a subgenre. We went to the spotify music hackathon, bright-eyed and bushy-tailed and ready for adventure. Though the hackathon itself was fab and the speakers were inspiring - we didn't figure out our subgenre problem. The million song dataset wouldn't fit on my AWS, _and_ the spotify API didn't return the things that we were interested in. The fields were empty. Additionally, the awesome python libraries that we could use to analyse audio files was not cooperating with my python3. All of these barriers would have been circumventable if only I believed in the cause. I said yes to this because I was excited to work with music but I classifying genres into subgenres was not something I felt I could put my heart into. And so all of these hiccups felt all the more monumental. With retrospect I could have done some pretty neat classification problems surrounding music:

Is this similar or not similar to your current taste in music and do you w

BOOM,

BOOM,

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

I did a feature importance analysis to pull out the most important ones, as a few of the features were mathematically related to each other and also there were 16. So it couldn't hurt to prune some out. Here was the result:

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/class_features.png?raw=true)

I took the top 5 and then used a bunch of classifier algorithms to try and find the most accurate classification system: Regression, SVM, Decision Tree, and Random Forest. All sci-kit learn fuelled. 

### A little summary of the classifiers

##### LOGISTIC REGRESSION


##### SVM

This bad boy is really hard to wrap your head around. SVM stands for __Support Vector Machine__ so you know it's going to be a bit abstract. It's based on the distance between vectors of the data. Wihout a __kernel__, they are only really useful for data that is linearly divided. It looks a little like this:

A kernel will increase the dimensionality, draw a line in the higher dimension space, and then reduce the dimensionality so that the line is no longer straight. Simple, right? When you use kernels correctly SVMs can be super useful for all kinds of classification problems. But as always you need to be careful of overfitting. 

##### DECISION TREES and RANDOM FORESTS

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/forest.png?raw=true)

These are probably my favourite classifier. They are super visual and really intuitive but also really effective in lots of situations. A decision tree consists of a starting point, where you have ALL your data, and then you split that data by asking a simple yes/no question(can be a question with more than two answers but the standard is 2) and splitting the data accordingly. For example, is your leaf longer than 10cm? Yes: contains all leaves longer than 10cm, and No: contains all leaves shorted than 10cm. You set how many questions you want to ask, or more properly, how many levels you want the tree to contain and _Voilá_! You end up with categories into which new leaves will be classified. With labeled data, the accuracy score is the probability that a leaf is correctly classified.

Random Forests just do the same process multiple times (lots of decision trees) and then take the mode or mean prediction of the sum of the trees. The good thing about Random Forests is that they can reduce the risk of overfitting that can happen with Decision Trees. 

And LOW AND BEHOLD, the Random Forest classifier was the most accurate for my leaf dataset. Which was just perfect. Because leaves. And trees. And forests. Geddit?

Here's the breakdown:

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/class_accuracy.png?raw=true)






