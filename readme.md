# **Finding Lane Lines on the Road**

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals/steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on the implementations that were created and how they affected the outcome



![](STEPS/Original.png?raw=true)Original


![](STEPS/STEP8.png?raw=true)Final

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 8 steps, but I think of my pipeline in three large steps.<br>
<br>
   First the feature extraction<br>
     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b> hsv_V </b> : only using the Value color channel in HSV to find lines<br>
     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>hsv_H</b> : only using the Hue color channel in HSV to find lines<br>
     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>Masked</b> : both hsv_V and hsv_H are masked together in grayscale <br>
   The idea is that there is always a color channel that can see the lane lines<br>
<br>
   Second cropping and editing if the image<br>
     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>gaussian_blur</b> : blurs the grayscale image<br>
     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>canny</b> : uses the blurred image to detect edges<br>
     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>region_of_interest</b> : masked only a polygon of the area of interest<br>
<br>
   Third finding & drawing lines.<br>
<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>hough_lines</b> : Identify where lanes are and apply it to the draw_lines function to create longer and more accurate lane lines.Uses draw_lines.<br>
              &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>draw_lines</b> : distinguish the X & Y between left and right lanes by finding the slope, average the position of each image return a black canvas with lines.Uses mvg_avg_left/mvg_avg_right.<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>mvg_avg_left</b> : Identify moving average for left lane<br>
                        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>mvg_avg_right</b> : Identify moving average for right lane<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<b>weighted_img</b> : overlays the lines to the original image<br>
<br>
<b>Draw lines</b> function takes in two variables and outputs a black canvous with lines.<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<u>How It Works</u> <br>
    <br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;First: Identify the left and right lane lines using the slope (if slope is negative then it's in the left, positive slope is in the right lane)then append the appropratie X & Y to its corresponding list.<br>
    <br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Second : Calculate and extended average lines by using intercept over avarage slope. <br>
    <br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Third: two functions(mvg_avg_right,mvg_avg_left) are used to keep and update overall moving averages. <br>
    


![](STEPS\Original.png?raw=true) Original
![](STEPS\STEP1.png?raw=true) hsv_V Output
![](STEPS\STEP2.png?raw=true) hsv_H Output
![](STEPS\STEP3.png?raw=true) Masked 
![](STEPS\STEP4.png?raw=true) Blur
![](STEPS\STEP5.png?raw=true) Edge
![](STEPS\STEP6.png?raw=true) Region_Of_Interest
![](STEPS\STEP7.png?raw=true) Draw lines
![](STEPS\STEP8.png?raw=true) Mask original with lines

### 2. Identify potential shortcomings with your current pipeline

There are a lot of potential shortcomings with my current pipeline.<br>
    Some of the big shortcomings are:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If the HSV filters cant see the lanes. None will be returned from hough lines function and pipeline will throw an error.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If the road condition is poor, edge detection could falsely identify lanes which could lead to unperdictable lines.<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Sharp turns and complex lane lines could cause errors and mistrack. 


### 3. Suggest possible improvements to your pipeline

One big improvement I can think of is to the apex of the mask. If the apex could adjust for turns on the road one could see further down the road. This could potentially fix the sharp turn shortcoming.
By adding more color filters or shape detections to the pipeline. The pipeline could be more accurate in low lighting or poor road conditions


### 4. Challenge Video 

In order to get the challenge video to work variables need to be changed in the following functions<br>
<br>
<b>process_image()</b>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;threshold = 40
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;minLineLength = 60
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;maxLineGap = 30
<br>
<b>draw_lines()</b>
<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;y_min = 450


