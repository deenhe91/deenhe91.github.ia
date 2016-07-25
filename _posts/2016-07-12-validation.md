---
layout: post
title: MODEL VALIDATION
excerpt: "leaky boots and cross bags"
categories: [Data Thoughts]
comments: true
image: 
  feature: http://66.media.tumblr.com/tumblr_lt4kzqtRxc1qa554ro1_1280.jpg
  credit: 
  creditlink: 

---


Being a good data scientist is not about knowing how to build a model. That's relatively easy, as soon as you know how to use `python` and `scikit learn` you're golden. You can build machine learning models to your heart's content. But that is useless if you can't interpret the results of that model. You get an accuracy score but _what does that mean_? If you have a high accuracy score for something that, logically, cannot possible be that accurate, then you need to be able to spot that and realise that you can't do your winner dance just yet. It is not easy to predict something. Humans are incredibly good at predicting things and we have process _11 billion_ bits of information every second. Of course we aren't aware of all this madness, but it allows that little bubble of blooming up consciousness to be somewhat meaningful. 

The models we build are relatively simple (in most cases), but often they are used to predict complex problems. And we should not expect an accuracy of 80 or 90% in most cases. So, first step, be aware of what your problem is. If you are predicting, _what_ are you predicting and imagine yourself predicting it. What would you yourself need to make a good estimate of whether something will happen or not. 

I was listening to the latest [Data Skeptic](http://dataskeptic.com/epnotes/predictive-models-on-random-data.php) episode today and Kyle interviewed this total boss, Claudia Perlich who spent the half hour leading an enlightening discourse on the world of model validation and data detective work. It was fascinating so I wanted to share some of that on here. Both for future reference, and potentially to pass on her wisdom to anyone else who is interested. 


### Leakage

Leakage in terms of machine learning, is the creation of unexpected additional information in the training data. Perlich and colleagues wrote [this paper](http://dstillery.com/wp-content/uploads/2014/05/Leakage-in-Data-Mining-Formulation-Detection-and-Avoidance.pdf) on the topic.


#### KDD-Cup 2008

Perlich and her team took part in a machine learning competition where the aim was to predict whether a patient had breast cancer based on mammography data.

They found that when including the randomly assigned 10-digit patient number to the model, the accuracy of the model improved by 30%. That's an insane improvement, especially considering the patient ID is supposed to have no bearing on the test outcome whatsoever. When digging deeper into the origins of the provided dataset, Perlich discovered that to ensure they had enough data for the competition, data was collected from four different centres - including both treatment centres and test centres. The randomly assigned ID numbers bore some trace of these centres and, as the likelihood of a patient having cancer is drastically different based on whether they are only being tested, or whether they are actually being treated, the model could just accurately predict which centre the patient image data came from and, in doing so, automatically increases the likelihood that they do or do not have cancer.

## _"the merge was done without obfuscating the source"_

Obfuscation means to confuse or obscure the original and intended meaning in the data. Without doing this, the labels to be predicted may unintentionally already be in the dataset, as in the previous example, and renders the model useless for real-world use. 

### Stacking

This was another super cool thing that I didn't previously know about. Sometimes, depending on the specific environment or situation of your problem, even though globally it's the same problem, the optimum model to use varies and yet may be equally valid. In this case you need to alter which model you use based on certain specifications. In stacking, you can have one model to predict which model would be best to use, and then another layer where you use the chosen model for your initial problem.

I hadn't thought of models in this way but of course machine learning can be embedded on many different levels, and to work in tandem. I hope to explore this idea further and blog up some examples in the near future.



