---

layout: post
title: words and power
excerpt: "the rhetoric of the Scottish National Party"
categories: [Metis Projects]
comments: true
image:
  feature: https://github.com/deenhe91/deenhe91.github.io/blob/master/images/edinburgh.png?raw=true
  credit: 
  creditlink:
---

#### Background

Natural Language Processing is one of the reasons I love data science. Perhaps because every level of it has some semantic value. There is information that is intricately tied to your upbringing, experience, perception of the world, in every word, every sentence. The data doesn't necessarily have to be large to be more meaningful to you. For example, 
an incredible poem with an enormous amount of information regardless of how many words are there. 

_"For sale: baby shoes, never worn." - Ernest Hemmingway_

And as the number of varying words we have available to us increases, the complexity increases, we can pick up currents of new information. Glean insights into crowd behaviours and perceptions. A window into the workings of an individual on one level, and into the workings of an entire organisation, country or culture on another level. 

Now I have waxed myself lyrical, I'll move on to the NLP project I undertook. 

I grew up in Scotland. And in recent years there's been some interesting controversy up there. Scotland became swallowed up by Britain in the 18th Century and hasn't fully recovered (emotionally) since. And though Scotland benefits financially from the union with England, politically and socially it is a very bad pairing. Scotland is a far more socialist country than England. It is also a country well-loved by left-leaning hippy types which makes it a beautiful, creative and low-key country. England has a population 10 times that of Scotland, and though I love England too, I find my love largely directed at London where diversity and creativity is at a peak. But the majority of England is politically right-minded.

So in 2014 Scotland held a long-awaited independence referendum. The sentiment was clear, the leave campaign was far louder but there were many of us who felt we were better together and devolution was a better choice. The Scottish National Party instigated the referendum and this party was led by Alex Salmond, a target for the media and a seemingly spiteful and resentable man. Leave lost, 49 to 51%. After this heavy disappointed for half of the country, Salmond resigned and Nicola Sturgeon took his place as leader of the SNP party. In the 8 months that followed, Sturgeon gained massive support from leavers and remainers alike and in the 2015 General Election the SNP won 97% of the seats in Scotland. This map clearly highlights the extent of her total domination of the country:

![](https://static.independent.co.uk/s3fs-public/thumbnails/image/2014/11/15/16/v2-sturgeon-pa.jpg)


#### The Question

My question is, how did she pull it off? And is it possible to see changes in the way that people talked about the SNP before and after the referendum and leading up to the election that meant that _even though_ the majority of the country had voiced their opinion as contradicting the __main__ incentive of the SNP, they still voted them in. Everywhere. 

My approach here was two-fold. I wanted to look at what Sturgeon and Salmond had said, and also at what the papers had said. 

#### Limitations

Now there were many constraints here that meant that these were limited. Time being the main one. Also, the transcripts of public speeches of Sturgeon and Salmond weren't as readily available as I'd hoped and the best thing I could find was the General Election debate which included Sturgeon and 5 other party leaders. The other was limited in that the Guardian is the only UK newspaper with an API. So my newspaper representation of the case was limited to that one newspaper.

#### The approach

I wanted some practice using Amazon Web Services so I decided to do all the newspaper stuff on an AWS instance using vim from my terminal. This involved setting up an instance and _ssh_ing into it whenever I wanted to work on that part of the project. I then used [`scp -i`](http://stackoverflow.com/questions/11304895/how-to-scp-a-folder-from-remote-to-local) to copy things from the instance to my local computer when I wanted to visualise or present. 

I used the Guardian API to request all links to pages containing content on __Scottish Independence__, __Scottish referendum__, __Nicola Sturgeon__ and __Alex Salmond__. This returned about 35,000 links which were categorised into articles, web blogs, video, images and interactive. I just wanted to use the articles, so I took the article links and fed them into a [Newspaper](http://newspaper.readthedocs.io/en/latest/) scraper that I built to collect the article text.

It kept tripping up and throwing an error so I introduced a neat little trick to keep it running. 

```python

for val in GuardianArticles['article']:
        try:
                a = Article(val[1])
                a.download()
                a.parse()
                a.nlp()

                article={ "url": val[1],
                        "text":a.text,
                        "keywords":a.keywords,
                        "authors" : a.authors,
                        "date" : val[0] }

                article_id = guardian_articles.insert_one(article)
                print(article_id)

        except:
                print('pass')
                pass


```

Using `try:, except: Pass`, when the scraper encountered an article that didn't fit the bill, it would just skip over it. I included `print('pass')` and a counter so I could see how many didn't work. Luckily it was a tiny percentage of the total number of articles. Some 40 links out of 18,000.

When plotted across a timeline we can see a that in general, the number of available articles on the topic is skewed towards more recent years. In addition, we can see a spike in the number of articles on the day of the referendum in 2014 and the general election in 2015. Just what would be expected.

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/articledistribution.png?raw=true)






