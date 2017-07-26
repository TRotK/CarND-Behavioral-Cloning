#**Behavioral Cloning** 

**Behavioral Cloning Project**

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/model.png "Model Visualization"
[image2]: ./examples/center_2017_07_24_22_54_23_728.jpg "Center Image"
[image3]: ./examples/center_2017_07_23_14_34_10_988.jpg "Normal Image"
[image4]: ./examples/flipped_center_2017_07_23_14_34_10_988.jpg "Flipped Image"
[image5]: ./examples/loss_viz.png "Loss Visualization"

## Rubric Points
###Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.  

---
###Files Submitted & Code Quality

####1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* model.py containing the script to create and train the model
* drive.py for driving the car in autonomous mode
* model.h5 containing a trained convolution neural network 
* writeup_P3_Li.md summarizing the results

####2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```sh
python3 drive.py model.h5
```
I have increased the speed to 20 mph in drive.py with the successful simulating run shown in video.mp4. It could also handle even higher speed while I did not test.

####3. Submission code is usable and readable

The model.py file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

###Model Architecture and Training Strategy

####1. An appropriate model architecture has been employed

The NVIDIA model is adopted with slightly modification, which consists of a convolution neural network with 5x5 and 3x3 filter sizes and depths between 24 and 64 (model.py lines 88-92) 

The model includes RELU layers to introduce nonlinearity (code line 88-95), and the data is normalized in the model using a Keras Lambda layer (code line 86) and cropped using a Keras Cropping2D layer (code line 87) 

####2. Attempts to reduce overfitting in the model

The model was trained and validated on different data sets to ensure that the model was not overfitting (code line 74-75). The model was tested by running it through the simulator and ensuring that the vehicle could stay on the track.

####3. Model parameter tuning

The model used an adam optimizer, so the learning rate was not tuned manually (model.py line 99).

####4. Appropriate training data

Training data was chosen to keep the vehicle driving on the road. I used the center lane driving, making use of the left and right camera as well. I also reflected the center camera with inversed steering measurements to augment data.

For details about how I created the training data, see the next section. 

###Model Architecture and Training Strategy

####1. Solution Design Approach

The overall strategy for deriving a model architecture was to start from simple networks and if it does not work, try more complicated ones. Augment the data set if needed.

My first step was to use a convolution neural network model similar to the LeNet. I thought this model might be appropriate because my dataset is relatively small. I started from the provided sample dataset with around 20000 images.

However, the model performed ok in the straightaway, but failed in the curves. So, I switch to a deeper model, i.e. NVIDIA model. I also collected some my own data and added it into the original dataset.

In order to gauge how well the model was working, I split my image and steering angle data into a training and validation set. I found that my first model had a low mean squared error on the training set but a high mean squared error on the validation set. This implied that the model was overfitting. 

To combat the overfitting, I removed the last but one fully-connected layers from the NVIDIA model.

The final step was to run the simulator to see how well the car was driving around track one. There were a few spots where the vehicle fell off the track. To improve the driving behavior in these cases, I collected a little more data in the curves.

At the end of the process, the vehicle is able to drive autonomously around the track without leaving the road.

####2. Final Model Architecture

The final model architecture (model.py lines 84-97) consisted of a convolution neural network with the following layers and layer sizes.

Here is a visualization of the architecture (note: visualizing the architecture is optional according to the project rubric)

![alt text][image1]

####3. Creation of the Training Set & Training Process

To capture good driving behavior, I first recorded two laps for counter-clockwise and clockwise, respectively,  on track one using center lane driving. Here is an example image of center lane driving:

![alt text][image2]

I did not recorded the vehicle recovering from the left side and right sides of the road back to center since this is not legal to do for training real self-driving cars as we are taught in class.

To augment the data set, I also flipped images and angles thinking that this would let the network learn symmetrically. For example, here is an image that has then been flipped:

![alt text][image3]
![alt text][image4]

After the collection process, I had 58,128 data points. I then preprocessed this data by normalization and cropping.

I finally randomly shuffled the data set and put 20% of the data into a validation set. 

I used this training data for training the model. The validation set helped determine if the model was over or under fitting. The ideal number of epochs was 7 as evidenced by training and validation loss not decreasing. I used an adam optimizer so that manually training the learning rate wasn't necessary.

The figure below visualizes the outputting training and validation loss.

![alt text][image5]
