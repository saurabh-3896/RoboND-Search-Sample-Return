## Project: Search and Sample Return
### Writeup Template: You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

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

[image10]: ./misc/matplotlib.png
[image11]: ./misc/processing.png
[image2]: ./misc/output.png
[image3]: ./misc/final_output.png

## [Rubric](https://review.udacity.com/#!/rubrics/916/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it!

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images (first with the test data provided, next on data you have recorded). Add/modify functions to allow for color selection of obstacles and rock samples.
The default notebook gives a good idea of what to modify and add. Firstly, I modified the color_thresh function to threshold navigable terrain and obstacles inverting the threshold values. Alternative was to complement the navigable terrain. The interactive mode of matplotlib allowed to fing the RGB color ranges for terrain, obstacles and rocks.

![alt text][image10]

After combining all the proces steps like thresholding, finding coordinates, rotation and translation we obtain the following output. The final image can be used to determine the navigable angle. It is calculated using to_polar_coords() function.

![alt text][image11]

#### 1. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result.

After verifying that all functions produce desirable output, it's time to populate them. This is accomplished in the process_image() function. The terrain, obstacles and rock map are converted to worldmap coordinates and overlayed on the ground truth. The mosiac image is formed and using moviepy output video is generated.


![alt text][image2]
### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) functions in the autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they were.
The functions used in the Notebook Analysis steps are available perception.py. The task remaining used to combine them as we did process_image(). The data was accessed using the Rover object.

One issue I was facing was that though the code looked correct the rover was behaving as expected.Poor fidelity. After going throught the discussion forum I realised that the issue was because of the camera Calibration. it was done at a particular value of the gyroscope and when the rover moved on exteme terrains the logic of perception failed.

Finally by trial and error I added condition to check if roll and pitch was in particular range before passing image to worldmap.This improved the fidelity many folds.
      "
      if roll <= 0.7 or roll >= 359.5:
            if pitch <= 0.7 or pitch >= 359.5:
              pass
      "

#### 2. Launching in autonomous mode your rover can navigate and map autonomously.  Explain your results and how you might improve them in your writeup.  

**Note: running the simulator with different choices of resolution and graphics quality may produce different results, particularly on different machines!  Make a note of your simulator settings (resolution and graphics quality set on launch) and frames per second (FPS output to terminal by `drive_rover.py`) in your writeup when you submit the project so your reviewer can reproduce your results.**

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

One of the possible reason for the pipeline to fail is if the enviroment changes. That is if the threshold levels for terrain,obstacles and rocks change. One solution can to add Calibration for these as well along with camera. Using deep learning based segmentation can be more helpful in real world applications.

I used Ubuntu 18.10 and did not find any option to change the resolution. I have used the default settings



![alt text][image3]
