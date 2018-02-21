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
[image4]: ./output/Image4.png
[image5]: ./output/Image5.png
[image6]: ./output/Image6.png

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

Navigable terrain, obstacle and rock image respectively.

#### 2. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on test data acquired by recorded training mode using the `moviepy` functions provided to create video output. 

1) Define source and destination points for perspective transform
Source and destination is using the same as above and references using grid image pixel.

2) Apply perspective transform
using perspect_transform to get navigable area and mask area (area of camera view)
warped = perspect_transform(img, source, destination)
mask = perspect_transform(np.ones_like(img[:,:,0]), source, destination)

3) Apply color threshold to identify navigable terrain/obstacles/rock samples

threshed = color_thresh(warped)

obs = mask - threshed

rock = find_rocks(img)

Navigable area is defined by thresholded colour RGB which have value more than 165 each component. The output in binery image.

Obstacle area is defined by mask area subtract by binary navigable area. And rock get from function find_rocks using HSV selection color.

4) Convert thresholded image pixel values to rover-centric coords
Using rover_coords function to get rover centric coordinate of navigable area and obstacle

5) Convert rover-centric pixel values to world coords
Each rover-centric coordinate of navigable area and obstacle than maps to world coordinate using pix_to_world function

6) Update worldmap (to be displayed on right side of screen)
Update data, data.worldmap component which is obstacle located in Red component, and navigable terrain in Blue component.

If rock available, then image get from find_rocks converted to rover coordinate and then mapped to world coordinate. Rock image put in data.world green component.

Video result of this jupyter notebook program as you can see in this picture bellow :

![video result][image4]


### Autonomous Navigation and Mapping

#### 1. Modification of perception_step() and decision_step()
##### perception_step()
In perception step, the function that we get from repositories is still empty. Therefore we need to modify the function to make the rover can get information of navigable terrain, rock and obstacle from camera and then make decision and action. There are seven standard step that have been described in Notebook analysis that applied that is perspective transform, thresholding, find rock, update rover vision, convert  image vision into rover centric coordinate and then convert into world coordinate.

Rover would be move in direction of angle polar coordinate get from mean navigable terrain.

The new thing is if there has a rock detected, then inform to pick_up the rock in direction of angle location of rock. Update the near_sample as True, and when the distance closed < 6, Rover then turn into stop mode and send_pickup to be true.

##### decision_step()
In decision_step, the changed value have been made in set steering range +/- 15 into +/- 10. This changed is due to, when the rover in area of big navigable space in autonomous mode, the rover only make a circle move.

Another changed is on forward mode when rock detected. As the rover navigation angle when rock detected is angle of rock then the value of Rover.nav_angles would be less than Rover.stop_forward. Therefore I was add or logic Rover.near_sample to the operation. Then the value would be true when rock detected

#### 2. Launching in autonomous mode that rover can navigate and map autonomously.  The results and how you might improve them in your writeup.  

![Result Image][image5]
When in autonomous mode without picking up the stone, the program can mapped and detect rock in almost completed. But when pick_up rock activated, I have a problem the rover could not move again after pick the rock as a picture bellow. I must optimise the program therefore the rover can mapping the navigable terrain and can collect the rock that spread in the maps.

![Result Image Pick Up Stone][image6]
