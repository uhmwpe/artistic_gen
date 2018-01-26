##Neural Network Style Recreation and Transfer

A repository of code for applying different styles of paintings and drawings to other images. The long term goal is to find a way to do this style transfer efficiently so that it can be effectively applied to videos, which are just a high amount of photos. Further details of implementation will follow, and progress on this repository will be updated periodically

##Status

Completed neural network style transfer

##TODO

Blurring of Photos to low resolution (some form of max-pooling or average-pooling will be experimented with)

Super Resolution

Experimenting with RNN or LSTM network in order to take advantage of the sequential order of movie files

##Neural Network Style Transfer

Neural Network Style Transfer contains 2 essential parts. The first part is to be able to recreate the physical form of the base image such that it isn't overshadowed by the style image. The second part is to be able to apply the style, which includes the colors and all the gradients, with as little physical form or shape as possible. We begin with content recreation

##Content Recreation

We take this basic photo of a truck:

![Alt Text](https://github.com/uhmwpe/artistic_gen/blob/master/images/truck.png)

And we pass it through the 2014 ImageNet winner, the VGG model, and model it's loss after it has been passed through a number of convolutional layers such that we don't recreate the exact picture, but maintain the important features as determined by the imagenet VGG filters. The result of the rendering is as shown

![Alt Text](https://github.com/uhmwpe/artistic_gen/blob/master/images/content_recreate.gif)
