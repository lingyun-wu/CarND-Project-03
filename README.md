# **Udacity Self Driving Car Nanodegree Project 3 -- Behaviorial Cloning Project**


Overview
---
This repository contains starting files for the Behavioral Cloning Project.

In this project, you will use what you've learned about deep neural networks and convolutional neural networks to clone driving behavior. You will train, validate and test a model using Keras. The model will output a steering angle to an autonomous vehicle.

We have provided a simulator where you can steer a car around a track for data collection. You'll use image data and steering angles to train a neural network and then use this model to drive the car autonomously around the track.

We also want you to create a detailed writeup of the project. Check out the [writeup template](https://github.com/udacity/CarND-Behavioral-Cloning-P3/blob/master/writeup_template.md) for this project and use it as a starting point for creating your own writeup. The writeup can be either a markdown file or a pdf document.

To meet specifications, the project will require submitting five files: 
* model.py (script used to create and train the model)
* drive.py (script to drive the car - feel free to modify this file)
* model.h5 (a trained Keras model)
* a report writeup file (either markdown or pdf)
* video.mp4 (a video recording of your vehicle driving autonomously around the track for at least one full lap)

This README file describes how to output the video in the "Details About Files In This Directory" section.

Creating a Great Writeup
---
A great writeup should include the [rubric points](https://review.udacity.com/#!/rubrics/432/view) as well as your description of how you addressed each point.  You should include a detailed description of the code used (with line-number references and code snippets where necessary), and links to other supporting documents or external references.  You should include images in your writeup to demonstrate how your code works with examples.  

All that said, please be concise!  We're not looking for you to write a book here, just a brief description of how you passed each rubric point, and references to the relevant code :). 

You're not required to use markdown for your writeup.  If you use another method please just submit a pdf of your writeup.

The Project
---
The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior 
* Design, train and validate a model that predicts a steering angle from image data
* Use the model to drive the vehicle autonomously around the first track in the simulator. The vehicle should remain on the road for an entire loop around the track.
* Summarize the results with a written report

### Dependencies
This lab requires:

* [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit)

The lab enviroment can be created with CarND Term1 Starter Kit. Click [here](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) for the details.



#### 1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* model.py [https://github.com/lingyun-wu/CarND-Project-03/blob/master/model.py] containing the script to create and train the model
* drive.py [https://github.com/lingyun-wu/CarND-Project-03/blob/master/drive.py] for driving the car in autonomous mode
* model.h5 [https://github.com/lingyun-wu/CarND-Project-03/blob/master/model.h5] containing a trained convolution neural network 
* writeup_report.md []or writeup_report.pdf summarizing the results

#### 2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```sh
python drive.py model.h5
```

#### 3. Submission code is usable and readable

The model.py file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

### Model Architecture and Training Strategy

#### 1. An appropriate model architecture has been employed

I used the Nvidia Convolutional Neural Network as my model architecture, which is showed in the image below.
![alt text][image1]

Here is my model summary
```
Number#      Layer (type)               Input Shape            Output Shape          
=======================================================================================
1            Lambda                     (66, 200, 3)           (66, 200, 3)
_______________________________________________________________________________________
2            Conv2D (with Relu)         (66, 200, 3)           (31, 98, 24)
_______________________________________________________________________________________
3            Conv2D (with Relu)         (31, 98, 24)           (14, 47, 36)
_______________________________________________________________________________________
4            Conv2D (with Relu)         (14, 47, 36)           (5, 22, 48)
_______________________________________________________________________________________
5            Conv2D (with Relu)         (5, 22, 48)            (3, 20, 64)
_______________________________________________________________________________________
6            Conv2D (with Relu)         (3, 20, 64)            (1, 18, 64)
_______________________________________________________________________________________
7            Flatten                    (1, 18, 64)            (1164)
_______________________________________________________________________________________
8            Dense (with Relu)          (1164)                 (100)
_______________________________________________________________________________________
9            Dropout (0.5)
_______________________________________________________________________________________
10           Dense (with Relu)          (100)                  (50)
_______________________________________________________________________________________
11           Dropout (0.5)
_______________________________________________________________________________________
12           Dense (with Relu)          (50)                   (10)
_______________________________________________________________________________________
13           Dropout (0.5)
_______________________________________________________________________________________
14           Dense (with Relu)          (10)                   (1)   
```

The model includes RELU layers to introduce nonlinearity, and the data is normalized in the model using a Keras lambda layer. 

#### 2. Attempts to reduce overfitting in the model

The model contains dropout layers in order to reduce overfitting and I only used 5 epochs for the first time training and 3 epochs for refinement training. 

Here are model mean squared error loss change figures for these two trainings

![alt text][image2]
![alt text][image3]

#### 3. Model parameter tuning

The model used an adam optimizer, so the learning rate was not tuned manually.


#### 4. Creation of the Training Set

The simulator produces three images per frame in the training model. Besides the center image, I also included images from left and right mounted cameras as the data set, which are adjusted by +0.275 for left image and -0.275 for right image. The sample images are showed below

![alt text][image4]  
![alt text][image5]  
![alt text][image6]
 
I also flipped images and angles thinking that this would balance the number of angles of left turn and right turn.


#### 5. Training Process

I split my sample data into 80% training and 20% validation data.

I first recorded two laps of center lane driving and one lane which focused on driving smoothly around curves. Because I'm not very good at driving a car in the game, the car in my recorded video always fluctuates around the center lane. This leads to poor autonomous driving results, in which the car fell off the edge of the first curve.

I then recorded the vehicle recovering from the edge where it fell off in the test run and trained the model again.

Every time the car fell off an edge or stuck somewhere in the track, I recorded the process of vehicle recovering from the edge. This procedure increased the number of the final training data set.

To sum up, I first trained the model with the initial data set, which contains two laps of center lane driving and one lap focusing on curves driving, for 5 epochs. Then, based on this trained model, I collected some data which can help the vehicle recovers from edges. At last, I refined the model with 3 epochs and finally the car could stay on the drivable lane for the whole lap.

The video in the repository is recorded with speed of 20 MPH.  



