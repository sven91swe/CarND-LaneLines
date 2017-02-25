**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

To run my code:
1. Run "Helper functions"
2. Run "Build a Lane Finding Pipeline"

This will generate the copies of the sample picture with drawn red lines on the lane lines.

3. Run all cells in "Test of Videos"

The jupyter notebook has been edited so that most of the tips are removed.

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps.

1. Turning the image into grayscale using the function grayscale
2. Blurring the image using the function gaussian_blur
3. Masking large portion of the image and leaving the region of interest using the function region_of_interest

4. Using modified hough_lines function that still uses the same openCV function but then filters out certain lines and merge the rest into the two lines that indicate the detected position of the line markers. This is done by for each line I get from the openCV function calculate the slope, k, and offset, m, in the form x = ky + m. Then I exlude every line detected where abs(k) > 2.5 as this removes lines that are a lot more horizontal than vertical in the picture. Horizontal lines are probably not lane marking. Then depending on the sign of k I assign every small line to the left or right lane marking. So now I have 2 sets of smaller lines. One for each lane line(right and left). I average the value of k and m in the set and create new lines from the by calculating the x when y is somewhere in the middle and at the bottom of the image.

5. I use a slightly modified version of draw_line to draw the lines. The modification is a small change in what the format of the input should be.

###2. Identify potential shortcomings with your current pipeline

I can now identify two major shortcomming with my current pipeline.

1. It can not handle curved lines. Even if I find parameters so that I detect the lane markings using the slope, k, is probalby not a good method for detecting if the line is to the right or left.

2. It can not handle straight lines if the car is not oriented into the direction of the lane. This is due to the fact that I am using the slope, k, to exlude detected horizontal lines that appear in certain frames.


###3. Suggest possible improvements to your pipeline

There are several possible improvement I can think of:

Instead of just using the sign of k for each detected part of a line I could form some form of histogram and say that the two lane lines are probably the two peaks.

My pipeline is currently looking at one frame at the time and not using any previous information. One way could be to set up a model of what is expected in the next frame in order to use information from more than one frame. Either to exlude certain input or to more smothly change the output.

