# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./images_writeup/original.png "Original Image"
[image2]: ./images_writeup/grayscale.png "Grayscale"
[image3]: ./images_writeup/grayscale_blur.png "Grayscale Blured"
[image4]: ./images_writeup/canny.png "Canny Edges"
[image5]: ./images_writeup/canny_ROI.png "Canny Edges within ROI"
[image6]: ./images_writeup/hough_lines.png "Hough Lines"
[image7]: ./images_writeup/hough_lines_post.png "Hough Lines with Post Processing"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of six steps. First, I converted the original image (figure 1) to grayscale (figure 2). 

![Original image][image1]

![Converted to grayscale][image2]

Then I applied a gaussian blur mask with a kernel size of 5 (figure 3). 

![Converted to grayscale and blurred][image3]

Subsequently, a canny edge filter (low_threshold = 60, high_threshold = 150) is applied (figure 4). 

![Canny edges][image4]

Only a region of interest (ROI) that roughly includes the lane in front of the ego vehicle is cut out (figure 5) and processed further.

![Canny edges within a ROI][image5]

Afterwards, the resulting images is converted to hough space to identify the lines (figure 6). Fine-tuning of the threshold parameters for this step was crucial for a robust identification.

![Lines identified in hough space][image6]

At this point the left and right line still consists of multiple parts. In order to draw a single line on the left and right lanes, I modified the draw_lines() function by splitting the lines to a left and right group based on their slope. For each group the median slope was calculated as well as the median intersection with the y-axis. The resulting lines are then returned (figure 7).

![Lines identified in hough space with post process][image7]


### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when the line has no great contrast compared to the concrete in the grayscale image. This might happen for example in very bright environments.  

Another shortcoming could be arise in very curved roads. The limited ROI would make it impossible to identify those lines.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to not only look at the grayscale image, but to use all color channels.  

Another potential improvement could be to dynamically change the ROI based on the last findings. It would also be possible to smooth the detection over various frames to decrease detection errors. 
