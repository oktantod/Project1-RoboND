## Project: Search and Sample Return
### First Project Submission of Term 1 Robotics Software Engineer Nanodegree Program Udacity 
### Oktanto Dedi Winarko

---


**The goals / steps of this project are the following:**  

**Training / Calibration**  

* Download the simulator and take data in "Training Mode"
* Test out the functions in the Jupyter Notebook provided
* Add functions to detect obstacles and samples of interest (golden rocks)
* Fill in the `process_image()` function with the appropriate image processing steps (perspective transform, color threshold etc.) to get from raw images to a map.  The `output_image` you create in this step should demonstrate that your mapping pipeline works.
* Use `moviepy` to process the images in your saved dataset with the `process_image()` function.  Include the video you produce as part of your submission.

**Autonomous Navigation / Mapping**

* Fill in the `perception_step()` function within the `perception.py` script with the appropriate image processing functions to create a map and update `Rover()` data (similar to what you did with `process_image()` in the notebook). 
* Fill in the `decision_step()` function within the `decision.py` script with conditional statements that take into consideration the outputs of the `perception_step()` in deciding how to issue throttle, brake and steering commands. 
* Iterate on your perception and decision function until your rover does a reasonable (need to define metric) job of navigating and mapping.  

[//]: # (Image References)

[image1]: ./output/Image1.png
[image2]: ./output/Image2.png
[image3]: ./output/Image3.png
[image4]: ./output/Image2.png
[image5]: ./output/Image2.png
[image6]: ./output/Image2.png
[image7]: ./output/Image2.png

## [Rubric](https://review.udacity.com/#!/rubrics/916/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### README

### Notebook Analysis
#### 1. Color selection of navigable terrain, obstacles and rock samples
My program using calibration grid and rock that provided by udacity repositories. This is the result of image :
![example of grid image, and example of a rock image][image1]

After we get grid image, than define source and destination for perpective transform (perspect_transform). Code for perspect_transform nothing changes. Source and destination for first time is used for find mask area that is the area of view camera.

Navigable terrain we can get from an image that we have received from rover camera and we doing perspective transform from it. Obstacles we can get from mask area subtract by navigable terrain. Image that we can get from this process that is:

![image terrain, image mask, and image rock][image2]

To get rock, we add function find_rocks, which is using HSV to determined yellow color in image.
	lower_yellow = np.array([20, 100, 100])
    	upper_yellow = np.array([100, 255, 255])
Color decision using opencv built in function inRange. The result of image in black and white color we can look as below :

![bw image terrain, image mask, and image rock][image3]

Navigable terrain, obstacle and rock image.

#### 1. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result. 
And another! 

![alt text][image2]
### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) functions in the autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they were.


#### 2. Launching in autonomous mode your rover can navigate and map autonomously.  Explain your results and how you might improve them in your writeup.  

**Note: running the simulator with different choices of resolution and graphics quality may produce different results, particularly on different machines!  Make a note of your simulator settings (resolution and graphics quality set on launch) and frames per second (FPS output to terminal by `drive_rover.py`) in your writeup when you submit the project so your reviewer can reproduce your results.**

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  



![alt text][image3]


