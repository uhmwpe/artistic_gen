## Neural Network Style Recreation and Transfer

A repository of code for applying different styles of paintings and drawings to other images. The long term goal is to find a way to do this style transfer efficiently so that it can be effectively applied to videos, which are just a high amount of photos. Further details of implementation will follow, and progress on this repository will be updated periodically

## Dependencies

To install the dependencies, there are 2 commands you must run

Pip requirements:
    pip install -r pip_requirements.txt

Conda requirements:
    sudo sh conda_install_packages.sh

And the python version used is 	python 3.6.3. It is recommended to start a new environment for this using the following code before running the above setup scripts

    conda create -n py36_test -y python=3.6 jupyter

Note that the tensorflow backend for keras was used, since the default installation uses the Theano backend, to change it, one must enter the /.keras directory, and modify the keras.json file such that backend no longer corresponds to theano, and instead, corresponds to tensorflow

Final note: imsave sometimes runs into errors and throws a 'module cannot be found' error, in such an event, restarting the kernel should fix the problem provided scipy and pillow were appropriately installed
    

### Status

Completed neural network style transfer

### TODO

Blurring of Photos to low resolution (some form of max-pooling or average-pooling will be experimented with)

Super Resolution

Experimenting with RNN or LSTM network in order to take advantage of the sequential order of movie files

## Neural Network Style Transfer

Neural Network Style Transfer contains 2 essential parts. The first part is to be able to recreate the physical form of the base image such that it isn't overshadowed by the style image. The second part is to be able to apply the style, which includes the colors and all the gradients, with as little physical form or shape as possible. We begin with content recreation

### Content Recreation

We take this basic photo of a truck:

![Alt Text](https://github.com/uhmwpe/artistic_gen/blob/master/images/truck.png)

And we pass it through the 2014 ImageNet winner, the VGG model, and model it's loss after it has been passed through a number of convolutional layers such that we don't recreate the exact picture, but maintain the important features as determined by the imagenet VGG filters. The result of the rendering is as shown

![Alt Text](https://github.com/uhmwpe/artistic_gen/blob/master/images/content_recreate.gif)

### Style Recreation

We take the most famous style for neural network style recreation: Van Gogh's Starry Night

![Alt Text](https://github.com/uhmwpe/artistic_gen/blob/master/images/starry_night.jpg)

and once again, we pass it through convolutional layers, but this time, we do not care to keep the physical form, but instead the style of the photo. The result of the rendering is as shown

![Alt Text](https://github.com/uhmwpe/artistic_gen/blob/master/images/style_recreate.gif)

## Combined Together

This way, we take the Van Gogh style and the picture of the rabbit below, and combine the two

![Alt Text](https://github.com/uhmwpe/artistic_gen/blob/master/images/original.png)

![Alt Text](https://github.com/uhmwpe/artistic_gen/blob/master/images/starry_night.jpg)

And we attain

![Alt Text](https://github.com/uhmwpe/artistic_gen/blob/master/images/style_transfer.gif)

Here we can see that the physical form of the original photo (and much of its bright color scheme) dominate the photo, and while it provides an aesthetically pleasing result in the color of the rabbit, it is not exactly what we are looking for. We seek to apply a 0.8 scalar multiplier to the content loss function and apply it to the bottom example

### Matrix Example

We take this infamous painting "Son Of Man"

![Alt Text](https://github.com/uhmwpe/artistic_gen/blob/master/images/son-of-man.jpg)

and seek to apply the ever famous Matrix theme to it

![Alt Text](https://github.com/uhmwpe/artistic_gen/blob/master/images/matrix.jpg)

This is a much different style transfer as the colors of the matrix image no longer consists of consistent gradients, but have many huge jumps in the color spectrum between pixels (quick jumps between the distinctive green and black), which would provide a new challenge to the convolutional net due to it's method of summing up values of surrounding pixel values. Additionally, we sought to apply a humanoid character to this particular matrix filter to observe how the brighter humanoid outline in the matrix filter will be applied to the original image. We note the following photo we show has only gone through 5 iterations due to lack of computational resources

![Alt Text](https://github.com/uhmwpe/artistic_gen/blob/master/images/matrix_style_transfer.gif)

Here we see that the style transfer very successfully captures the human silhouette. We also see that the distinctinve numerical matrix background is beginning to form. However, we are still noticing that perhaps the base image is overbearing the style transfer, and perhaps for future iterations, we would like to further decrease the weight the content-loss has on the optimization problem.

## Technical Difficulties

As with any neural network problem, one of the biggest challenges was the selection of hyperparameters. Since the high computation load meant that it was difficult to perform a high number of experiments within the limited 36 hour timeframe, I wasn't able to find the ideal combination of the content loss and the style loss. I also did not have the opportunity to experiment too much with the convolutional layer by which we extracted the output, and that is something that needs further experimentation in the future. 

A clear difficulty that both the examples I have provided faced was that stylistic transfers tend not to work as dominantly on photos that have bright colors (as should be quite intuitive), so perhaps some experimentation regarding rendering the original photos black and white, or reducing the values in the RGB channels, would allow for better stylistic transfer. 

## Future Goals for This Project

Ideally, what we would be able to accomplish with this project is an efficient way of applying stylistic transfer to videos. However, it must be stressed that the computational load when using neural network style transfer is very high. As a result, my proposed method for doing this is to first blur the images using either max pooling or average pooling, then applying the stylistic transfer. I observed noticeably quicker times when using photos that were smaller ( 112x112 photo took 7 minutes for 10 iterations vs 800x500 45 minutes for 5 iterations using Tesla K80 GPU - p2.xlarge instance on aws server).

From there, the next logical step would be to apply super resolution to reform the transformed photos back to their original size using deconvolution (or convolutional transpose, depending on your preference for terminology) techniques. 

Finally, the last thing I would like to experiment with is to apply an RNN or a variant (likely an LSTM due to exploding/vanishing gradient issues) as to capture the sequential information found in videos. Likely, the plan would be to pass a sequent of images into the RNN structure, which would be fed into the VGG network. Exact details of the implementation are still to be determined

