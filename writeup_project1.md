# **Finding Lane Lines on the Road** 


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Pipeline description

My pipeline for detecting lane lines consisted of 7 steps:
1. Standardize the input image shape, because my functions assume a particular size of the image and wouldnt work if inputs given were of different sizes.
2. Convert the image to grayscale image
3. Apply gaussian blur of 5x5 grid size, to reduce noise in the image
4. Apply Canny edge detection for edge detection
5. Define a region of interest area and mask out lines outside of that region -> In our case, assuming the LIDAR camera position is fixed in the car, the region of interest would be the almost triangular portion ahead of the camera which would contain the lane lines.
For this I created a 4-edge polygon ahead of the camera as a region of interest, for a 960x540 image (the standardization in step 1 is to make images to this size).
6. Convert the disjoint lines into coordinates in hough space and create solid lines from these points.
7. Combine the output of step 6, which contains just solid lines for lane lines with rest of the image black with the original image using a weighted sum of the images. This generates the final image with solid lane lines marked in red.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by calculating slope and intercept of each of the left and right lanes. From the hough space output lines, I put conditions on the output to see which classify as left lane lines (mini lines) and which classify as right lane lines. Then created a list of slopes and a list of intercepts of left lanes (called as left_slope, left_intercept respectively) and a similar duo of lists for right lane (right_slope and right_intercept)
Then I average out the values in each of the four lists to get one value of each average left lane slope, average left lane intercept, average right lane slope and average right lane intercept. With these I build solid left and right lanes.


### 2. Potential shortcoming with my current pipeline


One potential shortcoming is when the brightness of the road is also pretty high making lane lines not very visible. In other words, when my low and high thresholds given for canny image transformation dont work exactly for edge detection.
Another shortcoming is detecting straight line lanes even with curved lanes.


### 3. Possible improvements to the pipeline

A possible improvement would be to not hardcode thresholds but derive them somehow from the image dynamically.

Another potential improvement is again to not hardcode polygon borders (for region of interest) or hardcode conditions in draw_lines function to classify as left or right lane. Would probably not work when the camera is not centered on the vehicle as the code assumes.
