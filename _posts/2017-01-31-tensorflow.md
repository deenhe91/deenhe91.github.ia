---

layout: post
title: your own image classification network
excerpt: retraining the tensorflow Inception Network in python
categories: [Data]
comments: true
image:
  feature: https://jalammar.github.io/images/google-tensorflow-android.jpg
  credit: 
  creditlink:
---

The Inception v3 network, is a tensorflow neural network trained to distinguish between 1000 different categories from ImageNet. It was trained for the ImageNet Large Visual Recognition Challenge using the data from 2012. For this I used Google's 'Tensorflow for Poets' tutorial.

You need to have [docker](https://docs.docker.com/docker-for-mac/) installed

Once we've downloaded the flower image set make, we have to make these images available by linking them virtually:
`docker run -it -v $HOME/tf_files:/tf_files  gcr.io/tensorflow/tensorflow:latest-devel`
This can be any image set in the form of:

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/training_format.png?raw=true)

We have to retrieve the code for the Inception v3 framework...

```python
cd /tensorflow
git pull
```

The image set for fire that I downloaded from ImageNet unsurprisingly contained a lot of fireplaces, and the smoke set contained a lot of tanks. Combat is a smokey affair.. But I wanted to avoid the model deciding that because an image contained a tank, it would score high for 'smoke' and because an image contained an empty fireplace, it would score high for 'fire'. To try and counteract this, I added a third miscellaneous category with lots of images of empty fireplaces and non-smoking tanks to try and balance out the over image set. The model ran for 400 iterations, completing with a `final test accuracy` of `94.4%`. I wanted to test it myself so downloaded a selection of images to run through the model and see what results it would give. Every time an image is passed through, the model returns a probability score for each category. So when I gave it this calming beach campfire image:

[](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/Campfire.jpg?raw=true)

these were the returned scores:

```bash

fire (score = 0.86540)
smoke (score = 0.12902)
misc (score = 0.00558)
```

whereas a burning cigarette:

[](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/cigarette.jpg?raw=true)

resulted in this:

```bash

smoke (score = 0.79379)
fire (score = 0.15999)
misc (score = 0.04622)

```

and a happy raccoon:
[](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/raccoon.jpeg?raw=true)


was not much of any of them:

```bash

smoke (score = 0.48583)
misc (score = 0.28314)
fire (score = 0.23103)
```

Unfortunately, it wasn't all good news.




