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
    xpix, ypix = rover_coords(threshed)
    xpix_obs, ypix_obs = rover_coords(obs)

Get rover centric coordinate of navigable area and obstacle

5) Convert rover-centric pixel values to world coords
    scale = 10
    x_world, y_world = pix_to_world(xpix, ypix, data.xpos[data.count], 
                                data.ypos[data.count], data.yaw[data.count], 
                                data.worldmap.shape[0], scale)
    
    x_world_obs, y_world_obs = pix_to_world(xpix_obs, ypix_obs, data.xpos[data.count], 
                                data.ypos[data.count], data.yaw[data.count], 
                                data.worldmap.shape[0], scale)

Methods above is used for converting from rover centric coordinate into world coordinate.

6) Update worldmap (to be displayed on right side of screen)
    data.worldmap[y_world_obs, x_world_obs, 0] = 255
    if rock.any():
        xpix_rock, ypix_rock = rover_coords(rock)
        x_world_rock, y_world_rock = pix_to_world(xpix_rock, ypix_rock, data.xpos[data.count], 
                                    data.ypos[data.count], data.yaw[data.count], 
                                    data.worldmap.shape[0], scale)
        data.worldmap[y_world_rock, x_world_rock, 1] = 255

    data.worldmap[y_world, x_world, 2] = 255


![alt text][image2]
### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) functions in the autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they were.


#### 2. Launching in autonomous mode your rover can navigate and map autonomously.  Explain your results and how you might improve them in your writeup.  

**Note: running the simulator with different choices of resolution and graphics quality may produce different results, particularly on different machines!  Make a note of your simulator settings (resolution and graphics quality set on launch) and frames per second (FPS output to terminal by `drive_rover.py`) in your writeup when you submit the project so your reviewer can reproduce your results.**

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  



![alt text][image3]


