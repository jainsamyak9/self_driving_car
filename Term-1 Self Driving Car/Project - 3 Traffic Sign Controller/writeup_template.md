# **Traffic Sign Recognition** 

## Writeup



---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/visualization.jpg "Visualization"
[image2]: ./examples/grayscale.jpg "Grayscaling"
[image3]: ./examples/random_noise.jpg "Random Noise"
[image4]: ./examples/placeholder.png "Traffic Sign 1"
[image5]: ./examples/placeholder.png "Traffic Sign 2"
[image6]: ./examples/placeholder.png "Traffic Sign 3"
[image7]: ./examples/placeholder.png "Traffic Sign 4"
[image8]: ./examples/placeholder.png "Traffic Sign 5"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/udacity/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the vanilla python commands and numpy library to calculate summary statistics of the traffic signs data set:

Number of images in training set is 34799
Number of images in validation set is 4410
Number of images in test set is 12630
The shape of a traffic sign image is (32, 32, 3)
The number of unique classes/labels in the data set is 43


#### 2. Include an exploratory visualization of the dataset.

Below is an exploratory visualization of the data set. The bar charts depict distribution of images per class in training, testing and validation datasets. It is clear that there is a wide variation in number of images available per class in each of these datasets. Therefore, it is logical to think that data augmentation using preprocessing techniques can help reduce this disparity and thereby result in improved performance of the network.



![alt text][image1]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

The original training dataset was first converted to grayscale because traffic signs can be uniquely distinguished by shapes in most cases. Moreover, the reference paper uses grayscale images to achieve a respectable classification accuracy. Normalization was later applied to the dataset to achieve zero mean and equal variance. This helps the optimization algorithm converge in lesser steps thereby saving computational time.

As can be seen from the visualization of test, training and validation data; the number of images per class varies greatly in each of the datasets. This means that there might be features in validation data, that are not fully encompassed in the training data. To overcome this issue, data augmentation is applied. The first step in augmentation is to make a dataset of images that are below a certain number in each of the classes. Specifically, classes with number of images less than 800 are selected to be augmented. For this purpose, images are randomly selected from each class and various transformations like random zoom, random translation, random rotation and histogram equalization are applied. Also, data is generated is done a way such that the final distribution of class vs count is relatively flat. Finally, the generated data is converted to grayscale and normalized before concatenating with the original training dataset.

Examples from this augmented training dataset are given in figure below.

![alt text][image2]

As a last step, I normalized the image data because ...

I decided to generate additional data because ... 

To add more data to the the data set, I used the following techniques because ... 

Here is an example of an original image and an augmented image:

![alt text][image3]

The augmented data set has: 46480 images in total and a minimum of 800 images per class. The number of images per class (800) was chosen by trial and error.

#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image   							| 
| Convolution 3x3     	| 1x1 stride, same padding, outputs 32x32x64 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 16x16x64 				|
| Convolution 3x3	    | etc.      									|
| Fully connected		| etc.        									|
| Softmax				| etc.        									|
|						|												|
|						|												|
 


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used Adam optimizer. The final settings utilized for training are given below:


#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:

training set accuracy of 97.1%
validation set accuracy of 93.1%
test set accuracy of 91.1%
To keep things manageable at the beginning, LeNet architecture (implemented in the LeNet lab) was selected. However, the validation accuracy was limited to 89-90%. Moreover, plotting the loss and accuracy plots revealed overfitting. To overcome this issue, two dropout layers were added. The keep probability for these dropout layers was tuned by trial and error. The loss and accuracy plots for the current architecture are given below. It can be observed that the transition of accuracy and loss as the number of epochs increase is smooth.


As mentioned earlier, the performance of LeNet for classifying characters is well documented. Also traffic signs are made of simple shapes like characters from which one can reasonably assume that LeNet would give satisfactory performance. Moreover, the final model results verify that the model is indeed working well.



### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![alt text][image4] ![alt text][image5] ![alt text][image6] 
![alt text][image7] ![alt text][image8]

The first and fourth image might be difficult to classify because it is in green background.

The second and third images are warped which might present another challenge to the network.

Moreover, all five images are very bright compared to the training dataset. This can limit the convnets ability to accurately classify test images.
#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| Stop Sign      		| Stop sign   									| 
| U-turn     			| U-turn 										|
| Yield					| Yield											|
| 100 km/h	      		| Bumpy Road					 				|
| Slippery Road			| Slippery Road      							|


The model was able to correctly guess 3 of the 5 traffic signs, which gives an accuracy of 60%.


#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 17th cell of the Ipython notebook.

For the first image, the model is sure that this is speed limit 30km/h sign (probability of 0.999999), and the image does contain speed limit 30km/h sign. The top five soft max probabilities were


| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| .60         			| Stop sign   									| 
| .20     				| U-turn 										|
| .05					| Yield											|
| .04	      			| Bumpy Road					 				|
| .01				    | Slippery Road      							|

For the second image, the model is sure that this is Speed limit (20km/h) sign (probability of 0.83). However, the image contains a stop sign. The top five soft max probabilities were

For the third image, the model is relatively sure that this is a road work sign (probability of 0.60), and the image does contain a road work sign. The top five soft max probabilities were

For the fourth image, the model is sure that this is a speed limit (20km/h) sign (probability of 0.99999). However, the image contains a speed limit (70km/h) sign. The top five soft max probabilities were

For the fifth image, the model is sure that this is a keep right sign (probability of 1), and the image does contain a keep right sign. The top five soft max probabilities were


### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
#### 1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?


