# Google Inception V3 for Caffe
## revision 2

## Introduction

This model is a replication of the model described in the [Rethinking the Inception Architecture for Computer Vision](http://arxiv.org/abs/1512.00567)

If you wish to train this model on ILSVRC2012 dataset remember to prepare
LMDB with 300px images instead of 256px. 


## Hardware and Training

Original implementation from paper uses 32 batch size for 100 epochs
using RMSProp with learning rate of 0.045. You need some Titan X or K40 with more
than 10GB of RAM. Provided train_val.prototxt uses batch_size = 22 and fits
into Titan X.

Please use [NVIDIA/caffe](https://github.com/NVIDIA/caffe) for training. I was UNABLE to achieve good results using regular Caffe (probably because of different BatchNorm implementation).


## Training on TINY SET

I have trained this model on ImageNet subset of 18 categories for 11 epochs using [NVIDIA/caffe](https://github.com/NVIDIA/caffe) branch 0.15.5:  

```
I0628 08:35:16.726322 26414 solver.cpp:362] Iteration 11704, Testing net (#0)
I0628 08:35:56.465996 26414 solver.cpp:429]     Test net output #0: acc/top-1 = 0.796649
I0628 08:35:56.466174 26414 solver.cpp:429]     Test net output #1: acc/top-5 = 0.962012
I0628 08:35:56.466193 26414 solver.cpp:429]     Test net output #2: loss = 1.17044 (* 1 = 1.17044 loss)
```
If you want to try it yourself you can find it [here](http://cb.cr/Yhgc). Remember that this link provides model trained using ONLY 18 categories.

## DIGITS

If you want to train this network using [NVIDIA/DIGITS](https://github.com/NVIDIA/DIGITS) compatible train_val.prototxt is provided in digits folder for your pleasure. 
Please be advised that currently DIGITS web interface doesnt allow you to set following parameters for solver that allowed me to achieve such good results on tiny set:

 * rms_decay set to 0.9
 * clip_gradients set to 80
 * weight_decay set to 0.0004 (currently DIGITS calculate it automatically)
 
You can force DIGITS to use these parameters hardcoding these values into [train_caffe.py](https://github.com/NVIDIA/DIGITS/blob/master/digits/model/tasks/caffe_train.py#L508)

Just paste this code:
```
solver.rms_decay=0.9
solver.clip_gradients=80
solver.weight_decay=0.0004
```

Also if you will use DIGITS to create "New Image Classification Dataset" be sure to set Image Encoding to None.
 
## Training on full ImageNet

Right now Im training it on full ImageNet set using provided solver.txt. I will publish it when it`s done.
 

## License

This model is released for unrestricted use.
