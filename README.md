# **Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

## Reflection

### The pipeline for finding lane lines on the road

**My pipeline consisited of 5 steps:**

1. Converted the images to grayscale, which can help decreasing the impact of lane lines in different colors and make the feature edge more standing out;
2. Gaussian smoothing to suppress noise and spurious gradients by averaging;
3. Using Canny edge detector to do edge detection;
4. Extract region of interest, we only focus on the edges in the trapezoid area at the bottom center of the image where the current lane lines will appear;
5. Draw the two lane lines, we took advantage of hough space to determine the lines made up by dots on the edges detected previously.

After these five steps, we will get a image with red lines, so the final process is put the lines on initial image.

**Find the single line on the left and right lanes**

After using hough transform to find lines that may lie on the lanes, we encounter a problem which is that some lane lines are dashed, so we need to merge and extend them into one complete line.

Here is what we did using average and extrapolate.

1. Calculate slope $m=(y2-y1)/(x2-x1)$ for every line.
2. When slope is negative, the line belong to left lane, other wise it belong to right lane.
3. For each line, we calculate slope and intercepts on axis.
4. Calculate the average slope and intercept for left and right lanes respectively.
5. Extend the line to the bottom of picture, using the node we can get a single line each for left and right lane.

### Shortcomings of current pipeline

One potential shortcoming would be what would happen when there is shadow of trees or buildings on the road. Under this circumstance, the edge detection is not robust. Then we have a lot of edges not belong to lane line, which will later makes the line average and extrapolate fail to work.

So we still need more advanced algorithm for edge detection.

### Possible improvement

One way to help eliminate impossible edge lines is to give a range of possible slope, so we elimiate those horizontal lines. And we could even do more to eliminate the lines lie on the inside the lane. But this method is not flexible enough. 

So another way is to use more complex machine learning method to detect the edges of lane lines.
