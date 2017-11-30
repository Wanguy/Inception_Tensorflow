# Inception_Tensorflow

[![Travis](https://img.shields.io/badge/Language-English-blue.svg)](https://github.com/Wanguy/Inception_Tensorflow) [![Travis](https://img.shields.io/badge/Python-3.6-brightgreen.svg)]() [![Travis](https://img.shields.io/travis/rust-lang/rust.svg)]()

## 介绍

基于Inception v3的TensorFlow模型再训练。

转移学习（Transfer learning），这意味着我们从一个已经接受预先训练的模型开始，对类似的问题进行再训练。从无到有的深度学习可能需要几天的时间，转移学习则可以在短时间内完成。

在Demo中需要使用TensorFlow，你可以参考 [tensorflow Github](https://github.com/tensorflow/tensorflow) 来安装。

这个Demo展示了通过简单训练识别图片的过程。

![Screen Shot 2017-11-30 at 18.11.40](/Users/sephiroth/Desktop/Screen Shot 2017-11-30 at 18.11.40.png)

（训练500次，300张特征图下的准确率）

## 目录

> Inception_Tensorflow
>
> > bottlenecks `放置训练生成的缓存`
> >
> > data `图片文件夹`
> >
> > inception `放置 Inception v3 模型`
> >
> > label.py `测试用python程序` 
> >
> > retrain.py `训练用python程序`

## 使用 TensorBoard

在开始训练之前，在后台启动`tensorboard`。TensorBoard是一个用于监测和检查的工具，包括在tensorflow中。你将使用它来查看训练进度。

```shell
tensorboard --logdir training_summaries &
```

> This command will fail with the following error if you already have a tensorboard process running:
>
> 当你已经启动tensorboard进程时，命令将报错：
>
> `ERROR:tensorflow:TensorBoard attempted to bind to port 6006, but it was already in use`
>
> 你通过下面语句终止当前进程：
>
> `pkill -f "tensorboard"`

## 查看 cretrain 帮助

你可以使用python命令查看帮助信息。

```shell
python -m retrain -h
```

## 训练

正如介绍中所说，ImageNet有数以万计的参数，可以区分多种不同的分类。我们只需训练网络中的最终层即可，因此训练过程将不会花费你太多的时间。

通过下面的代码开始你的训练：

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

>  `--how_many_training_steps` 的默认值为 4000。
>
> `--summaries_dir` 为可选参数, 用以传递参数给TensorBoard可视化信息。

这个脚本将下载一个经过预先训练的模型，你需要做的就是添加最终层以及你下载的特征图片用于训练。训练过程可以通过TensorBoard可视化。

经过一段时间的训练后你会得到`retrained_graph.pb`和`retrained_labels.txt`两个文件。

## 测试

```shell
python label.py [image]
```

## 许可

Copyright (c) 2015-2017 Wanguy. Released under GPLv3. 通过 [LICENSE.txt](https://github.com/Wanguy/Inception_Tensorflow/blob/master/LICENSE) 查看详细内容。