# Inception_Tensorflow


## Introduction

Retrain a tensorflow model based on Inception v3.

Transfer learning, which means we are starting with a model that has been already trained on another problem. We will then be retraining it on a similar problem. Deep learning from scratch can take days, but transfer learning can be done in short order.

In this demo you need install TensorFlow. You can reference [tensorflow Github](https://github.com/tensorflow/tensorflow).

The Demo perform an ability to identify images after simple training.

![Screen Shot 2017-11-30 at 18.11.40](/Users/sephiroth/Desktop/Screen Shot 2017-11-30 at 18.11.40.png)

(accuracy on 500 times training and 300 feature images)

## Contents

> Inception_Tensorflow
>
> > bottlenecks `Empty folder to cache the training`
> >
> > data `Image data folder`
> >
> > inception `Empty folder to restore the Inception v3 model`
> >
> > label.py `The python program for labeling` 
> >
> > retrain.py `The python program for training`

## Start TensorBoard

Before starting the training, launch `tensorboard` in the background. TensorBoard is a monitoring and inspection tool included with tensorflow. You will use it to monitor the training progress.

```shell
tensorboard --logdir training_summaries &
```

> This command will fail with the following error if you already have a tensorboard process running:
>
> `ERROR:tensorflow:TensorBoard attempted to bind to port 6006, but it was already in use`
>
> You can kill all existing TensorBoard instances with:
>
> `pkill -f "tensorboard"`

## Investigate the retraining script

You can run the script using the python command. Take a minute to skim its "help".

```shell
python -m retrain -h
```

## Training

As noted in the introduction, Imagenet models are networks with millions of parameters that can differentiate a large number of classes. We're only training the final layer of that network, so training will end in a reasonable amount of time.

Start your retraining with one big command:

```shell
python -m retrain \
  --bottleneck_dir=tf_files/bottlenecks \
  --how_many_training_steps=500 \
  --model_dir=tf_files/models/ \
  --summaries_dir=tf_files/training_summaries/"${ARCHITECTURE}" \
  --output_graph=tf_files/retrained_graph.pb \
  --output_labels=tf_files/retrained_labels.txt \
  --image_dir=tf_files/flower_photos
```

> the default value of  `--how_many_training_steps` is 4000.
>
> note the `--summaries_dir` option, sending training progress reports to the directory that tensorboard is monitoring.

This script downloads the pre-trained model, adds a new final layer, and trains that layer on the flower photos you've downloaded. You can view training process via TensorBoard.

It may take a while to finish the training After the training 'retrained_graph.pb' and 'retrained_labels.txt' will be generated.

## Testing

```shell
python label.py [image]
```

## License

Copyright (c) 2015-2017 Wanguy. Released under GPLv3. See [LICENSE.txt](https://github.com/Wanguy/Inception_Tensorflow/blob/master/LICENSE) for details.