# **Finding Lane Lines on the Road**

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

[image2]: ./test_images_output/base_picture.jpg "base_picture"

[image3]: ./test_images_output/detected_lines.jpg "detected_lines"

[image4]: ./test_images_output/detected_lanes.jpg "detected_lanes"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I use gaussian smoothing in order to reduce noise. These processes are preparations for edge detection.(The image below is after 2 steps)

![alt text][image2]

In the third step, I use canny function for detecting edges. Forth step is cutting image to apply next process in particular region that lane lines exist. Finally, I apply Hough transform to detecting lines in the edges. After passing these processes, combine image of lines and initial image is below.etected_lines.jpg "detected_lines"

![alt text][image3]

As you can see, some lines are lacked or separated. In order to draw a single line on the left and right lanes, I modified the draw_lines() function by avarage each slope and bias of lines of each side, and extended them to bottom and middle height of image. By doing this, separated lines of each side is avaraged as single lane line. (Final result is below)

![alt text][image4]

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the car gets angle against lane lines such as lane change or right/left turn.  In that case, the pipeline can't detect lane lines or shows inaccurate lines, because acutual lane lines go out of frame which I set at "region_of_interest" function.
Another shortcoming could arise when it is dark environment such as night or cloudy wheather. In these situation, it becomes harder to detect edges becouse of noisy image or loss of contrast.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to expand the "region_of_interest". When we do that, some incorrect lines may appear. So, instead of expanding region, we can use filters that combines lane lines of previous frame and current one (filtering  by change of slope, bias or start/end points) like lowpass filter. By using them detecting lane lines becomes easier and we can reduce detecting inaccurate lines.

Another potential improvement could be to use stronger noise filter and have thresholds make stricter for noisy image or loss of contrast. Then, lane lines may become harder to detected, but we can estimate lanes by using dead reckogning. When no lines detected, lane lines can be estimeted by previous detected lines and data of yawrate and velocity of the vehicle. By using them, lane lines detection may become stronger by change of environment.
