# **Project: Finding Lane Lines on the Road** 
### This project uses computer vision and image processing techniques to detect the Lane Lines that are in the visible range of the car. Following explained is an overview of different steps involved in building block of the entire Image Pipeline, potentila shortcomings in the current method and area of improvement

## Overview

**Following are steps involved in the Image Pipepline**

- Different Libraries are imported to acheive particular Computer Vision and Mathematical tasks which include openCV and numpy
- As a first step towards processing image, convert the image into a gray scale image so that all different color channels of Lanes are drop down to a single color channel. [cv2.cvtColor()](https://docs.opencv.org/2.4/modules/imgproc/doc/miscellaneous_transformations.html)
- As a part of preporcessing it always a good practice to make use of gaussian filter. Since we will be detecting edges to ultimately detected Lane Edges, gaussian smoothening helps in noise reduction. [cv2.GaussianBlur()](https://docs.opencv.org/3.1.0/d4/d13/tutorial_py_filtering.html)
- As mentioned earlier to find lanes, detecting edges of the lanes is one of the process in the image pipeline, to do so *Canny Edge Detection* is one of the different ways. [cv2.Canny()](https://docs.opencv.org/3.1.0/da/d22/tutorial_py_canny.html)
- This provied with and completed black image having all different edges on it, since we are interested in a particluar region, we will apply mask on that Region of Interest and discard all other edges. [cv2.fillPoly()](https://docs.opencv.org/3.0-beta/modules/imgproc/doc/drawing_functions.html)
- Finding lane lines, actualy means on implementing or drawing line on the lane lines detected by camera. To do so we will have to transform Image Space into a Parameter Space also called as *Hough Space*. This Transfromation is called as *Hough Trasnformation*. [cv2.HoughLinesP()](https://docs.opencv.org/3.0-beta/doc/py_tutorials/py_imgproc/py_houghlines/py_houghlines.html)
- During this tranfromation the detected edge pixels will be transformed into required lines in the form their end point coordinates.
- These endpoint coordinates are now used to draw lines on the original image. [cv2.line()](https://docs.opencv.org/3.0-beta/modules/imgproc/doc/drawing_functions.html)
- Here the endpoints will result into segments of different lines on the image. To convert these segments into a single line acros the Region of Interest, we need to seperate the line segments as per their slope to tell if they belong to left lane line or right lane line.
- After doing so we can fit the x and y coordinates of the line segment in the form of *y=mx+b* to find the value of slope *m* and constant *b*. [np.polyfit()](https://docs.scipy.org/doc/numpy-1.15.0/reference/generated/numpy.polyfit.html)
- once we know the value *m* and *b* of the resultant line for both left and right line we can plot the resultant line from end to end of region of interest. [np.poly1d()](https://docs.scipy.org/doc/numpy-1.15.1/reference/generated/numpy.poly1d.html)
- These now results into two bold lines on the image indicating resultant Lane Lines of road.

[//]: # (Image References)

[image1]: ./output_images/EdgeImage.png
[image2]: ./output_images/GaussianSmoothenedImage.png
[image3]: ./output_images/GrayScaledImage.png
[image4]: ./output_images/HoughLineImage.png
[image5]: ./output_images/MaskedEdgeImage.png
[image6]: ./output_images/OutputImage.png
[image7]: ./output_images/OriginalImage.png

## Illustration of Pipeline:

### Below you can check image outputs of original image at different stages of pipeline.
![alt text][image7]
![alt text][image3]
![alt text][image2]
![alt text][image1]
![alt text][image5]
![alt text][image4]
![alt text][image6]

** [Here](./test_videos_output/)/"solidWhiteRight.mp4" is the complete operation implemented on a video **


## Potential shortcomings with current pipeline:

- There are few problems that occur with roads with steep curves, [here](./test_videos_output/)/"challenge.mp4" is the implementation of the current image pipeline on the steep curve video. You can see the lines drawn are randomly placed on each frame of the video.
- Also, due to the end-to-end point selection during drawing lines, for small spots on road within visible range causes an error in the final line drawn on the lanes, you can check [this](./test_videos_output/)/"solidYellowleft.mp4" video and observe how lines get deflected at some points.

## Possible improvements to current pipeline:

As mentioned in the above shortcomings, instead of selecting the end-to-end points while drawing line, pipeline should select each line segment and draw each of them and extrapolating only that part which is missing between the line segments. This could also probably solve the random line generation in this [video](./test_videos_output/)/"challenge.mp4"
