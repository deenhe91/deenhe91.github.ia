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

## inceptions
The Inception v3 network is a tensorflow [convolutional neural network](http://colah.github.io/posts/2014-07-Conv-Nets-Modular/) trained to distinguish between 1000 different categories from ImageNet. It was trained for the ImageNet Large Visual Recognition Challenge using the data from 2012. Happily for us, it is possible to retrain this model to create our own image classifier for specific image tasks. 

### what is _retraining_?
We can retrain the inception model by keeping all the trained layers in tact bar the last one. The first layers are responsible for things like picking out edges and basic shapes. These are necessary and universal steps for CNNs used in image classification and take a long time and a lot of images to train. The final layer of the neural net is where you can train the neural net to distinguish between certain flowers or animals, etc. Even happilyer for us, retraining this last step is well-documented and doesn't take much time or data so we can get right to it. 

I used Google's [Tensorflow for Poets](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/?utm_campaign=chrome_series_machinelearning_063016&utm_source=gdev&utm_medium=yt-desc#1)  codelab as my guide for this because it's super good. 

### tensorflow for poets steps

#### one: docker
You need to have [Docker](https://docs.docker.com/docker-for-mac/) installed

#### two: the image set
We need to have a dataset to retrain on. The codelab provides a dataset of different flowers if you just want to try it out but don't have a specific idea. If you do, then organise your image files like this:

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/trainingset_format.png?raw=true)

With images belonging to specific categories in folders named after that category. I wanted to be able to detect fire and smoke in images, thinking that having this run on frames of CCTV footage could be a useful safety aid.
I used ImageNet to get my dataset. The image set for fire that I downloaded unsurprisingly contained a lot of fireplaces, and the smoke set contained a lot of tanks for some reason. I suppose combat is a smokey affair.. But I wanted to avoid the model deciding that because an image contained a tank, it would score high for 'smoke' and because an image contained an empty fireplace, it would score high for 'fire'. To try and counteract this, I added a third miscellaneous category and included lots of images of empty fireplaces and non-smoking tanks to try and balance out the overall image set.

#### three: link image set to docker 
In order for the docker image to use the image files for retraining, we need to give the docker image access to the relevant folder, in this case `tf_files`:

`docker run -it -v $HOME/tf_files:/tf_files  gcr.io/tensorflow/tensorflow:latest-devel`

#### four: get inception code
The we have to retrieve the code for the Inception v3 framework...

```bash
cd /tensorflow
git pull
```

#### five: retrain the model

```bash
# In Docker
python tensorflow/examples/image_retraining/retrain.py \
--bottleneck_dir=/tf_files/bottlenecks \
--model_dir=/tf_files/inception \
--output_graph=/tf_files/retrained_graph.pb \
--output_labels=/tf_files/retrained_labels.txt \
--image_dir /tf_files/<imageset folder>
```
This will set off the process. __It's that simple!__. This process takes a little while, maybe half an hour. It generates 'bottlenecks'. This is the term used for the layer just before the final output layer that we retrain to do the classification. The values are stored in `/tmp/bottleneck` so if you ever need to retrain again those values don't have to be calculated. 

By default, the script runs 4000 training steps, but this can be adjusted. At each iteration, 10 images are picked at random from the training set and fed into the final prediction layer. The predictions are checked for accuracy by comparing them to the actual labels and then the final layer's weights are updated accordingly through back-propogation. It's quite fun to watch the accuracy score change as they are printed during training. There are two types of accuracy, the __training accuracy__ and the __validation accuracy__. The validation accuracy is the important one. That's the one that looks at test images, i.e. labeled images that the model hasn't been trained on. The training accuracy is also important but a high score for that could mean the model is overfitting. Both the training and validation accuracies should be high before it is a reliable model.

The model ran for 4000 iterations, completing with a `final test accuracy` of `94.4%`. I wanted to test it myself so downloaded a selection of images to run through the model and see what results it would give. Google codelabs provides you will a little python script that you can just curl neatly into a file called `label_image.py` and store in the `tf_files` folder.

```python
import tensorflow as tf, sys

# change this as you see fit
image_path = sys.argv[1]

# Read in the image_data
image_data = tf.gfile.FastGFile(image_path, 'rb').read()

# Loads label file, strips off carriage return
label_lines = [line.rstrip() for line 
                   in tf.gfile.GFile("tf_files/retrained_labels.txt")]

# Unpersists graph from file
with tf.gfile.FastGFile("tf_files/retrained_graph.pb", 'rb') as f:
    graph_def = tf.GraphDef()
    graph_def.ParseFromString(f.read())
    _ = tf.import_graph_def(graph_def, name='')

with tf.Session() as sess:
    # Feed the image_data as input to the graph and get first prediction
    softmax_tensor = sess.graph.get_tensor_by_name('final_result:0')
    
    predictions = sess.run(softmax_tensor, \
             {'DecodeJpeg/contents:0': image_data})
    
    # Sort to show labels of first prediction in order of confidence
    top_k = predictions[0].argsort()[-len(predictions[0]):][::-1]
    
    for node_id in top_k:
        human_string = label_lines[node_id]
        score = predictions[0][node_id]
        print('%s (score = %.5f)' % (human_string, score))
```

```
# type 'exit' to exit Docker and then:
curl -L https://goo.gl/tx3dqg > $HOME/tf_files/label_image.py
```
I created a folder in `tf_files` called `test_images`, where I could store an image that I wanted to run through the model. Then I could restart docker linked to `tf_files`,

```
$ docker run -it -v $HOME/tf_files:/tf_files  gcr.io/tensorflow/tensorflow:latest-devel
``` 

and run the following command:

```
# In Docker
$ python /tf_files/label_image.py /tf_files/test_images/Campfire.jpg
```

Every time an image is passed through, the model returns a probability score for each category. So when I gave it this calming beach campfire image:

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/Campfire.jpg?raw=true)

these were the returned scores:

```bash
fire (score = 0.86540)
smoke (score = 0.12902)
misc (score = 0.00558)
```

whereas a burning cigarette:

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/cigarette.jpg?raw=true)

resulted in this:

```
smoke (score = 0.79379)
fire (score = 0.15999)
misc (score = 0.04622)

```

and a happy raccoon:

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/raccoon.jpeg?raw=true)

was not much of any of them (although that is some smokey fur...):

```
smoke (score = 0.48583)
misc (score = 0.28314)
fire (score = 0.23103)
```

I wanted to label a whole bunch of images to test the effectiveness of the model in multiple different areas, so I tweaked the code to run for all the images in the `test_images` folder by adding this line inside the tf.Session():

```python
for filename in os.listdir(sys.argv[1]):
    if filename.endswith(".jpg") or filename.endswith(".jpeg"): 
```

The code worked great, but also revealed some problems with the model. Images that contained a lot of white or light grey were labeled as having a high probability score for 'smoke', and despite the _fire_ dataset containing the most images (almost 2000), fire was not likely to be labeled correctly. Perhaps this was because the majority of fire images from ImageNet were close up of fires in hearths so images containing fire like this one...

![](https://github.com/deenhe91/deenhe91.github.io/blob/master/images/burningcar.jpeg?raw=true)

...got a 60% probability that it contained 'smoke' and only a 35% probability that it contained 'fire'. In practice perhaps this isn't a huge problem because if smoke or fire is detected that is enough of a safety measure.

I suppose this means some updating to the dataset may be in order! More to come...




