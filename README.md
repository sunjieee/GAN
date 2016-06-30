# GAN
Generate images via a Generative Adversarial Network (GAN)

## What is a GAN?

A GAN is method for discovering the underlying distribution of a dataset, that is, it is a method in the domain of unsupervised representation learning. Most commonly it is applied to image generation tasks. 

A GAN combines two Neural Networks, called a Discriminator and a Generator. Given some dataset, the Generator takes as input random noise, and attempts to produce something that resembles an item within the dataset. The discriminator takes as input both the real dataset and the artifical data produced by the Generator, and attempts to distinuish between the two. Both are trained in tandem. 

## How does this GAN work?

I heavily borrowed from a number of other implementations [ [1](https://github.com/jacobgil/keras-dcgan) [2](https://github.com/skaae/torch-gan) [3](https://github.com/aleju/cat-generator) ]. However, with the other implementations I could not produce (vaguely decent) images on a single CPU in a short time frame, so I took a new approach for training the Genereator (G) and Discriminator (D). Both G and D are DCNNs (deep convolutional neural networks), Batch Normalization is used for G but not for D, as in previous experiments [4](http://torch.ch/blog/2015/11/13/gan.html), I found Batch Normalization in D made D far too good at distinguishing artifical images from real images.

To ensure that neither G nor D become to good at their respective jobs, I first defined a margin of error (e) such that:
|training loss of G - training loss of D| < e , for each training batch. The exact implementation results in the lesser of the training of loss of G and D swapping at each successive training batch, resulting in neither becoming too powerful.

Training GANs is extremely tough, a lot of care has to be paid to the learning rate parameter (as well as other parameters), and takes a long time to get right.

## How to run

This uses Keras (for ML) and OpenCV (for image manipulation).

Run ```train.py``` with the additional flags:
- ```--path "A directory of jpg files used for training"```
- ```--batch_size int```
- ```--epochs int```
- ```--TYPE str``` "train" (for training) or "generate" (for when training is done and you want to create pretty pictures).
- ```--img_num int``` Used for number of images you want to generate when ```TYPE``` is "generate".


## Experiences

Overall this was an extermely fun side-project. I used a very small training set of about 400 images and even with a single CPU machine was able to generate face like shapes within a few minutes, and more detailed faces withing a few hours. I imageine there a huge number of ways to improve the training process to improve results. The limitation of 64x64 images means even after a long time images still look fairly distorted. I have now begun training on a dataset consisting of thousands of images, which will take substantially longer to train but will hopefully produce better results.

## I see VAE functionality in model.py, what gives?

This is something I would have liked to have implemented but didn't have time. Combining a Variational AutoEncoder (VAE) with a GAN is popular as they seem to smooth out the rough edges produced by just a GAN. You can read about VAE's here.

