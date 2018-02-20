# **Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


## Reflection

### 1. Describe my pipeline. 

My pipeline consists of several steps. 

1. Use `cv2.cvtColor()` to convert the image to grayscale.
2. Use `gaussian_blur()` to reduce image noise and prepare the image for finding adges.
3. Use `canny()` to detect edges.
4. Use `region_of_interest()` to define the region for detecting lane lines
5. Use `cv2.HoughLinesP()` to find lines in the defined region in step 4.
6. Use `draw_lines()` to process the lines found in step 5 and print the desired one left and one right line.
7. Mark the lane between the left and right line from step 6. (optional)

In order to draw a single line on the left and right lanes, I modified the `draw_lines()` function by steps below:

1. Define two slop ranges, one for right line, the other for left line. 
2. Calculate the slope and center position of each line. 
3. Sort each line into either right line group or left line group. Those lines with slope not in either group will be ignored.
4. Lines sorted in each group will be handled separately. Take right line group as example, if it is not empty, calculate the average slope and average center point of all lines, then I have the slope and one point of the final line.
5. Use the same bottom and top y positions of region defined in pipeline step4 as each final line's y positions of two ends, then I can calculate the the paired x position of each point.
6. Use x and y position of both ends of the line to draw the line.
7. in `draw_lines()` function, I also use the x and y position of both ends of both lines in step 6 to mark the path, If both left and right line groups are not empty (optional).

Finally, process all images and videos in their folders and save the files with found lines.



### 2. Identify potential shortcomings with my current pipeline

One potential shortcoming would be what would happen if no lines can be found in either line group. At first the code failed when no line can be found in video when I simply drew all lines from `cv2.HoughLinesP()`, especially when running code for challenge.mp4 and running code in Raspberry Pi realtime with camera. I was thinking to use the line from last frame if no line can be found in the current frame, but I gave up because this is not the case in real world. If no line can be detected for a while, this will mis-guide the user. So I decided to draw lines only when lines can be found. But there might be some other ways to make it smarter.

Another shortcoming could be that I am using a fixed region of interest to find lines. There should be a smart way to adjust the region for different roads and conditions.



### 3. Suggest possible improvements to my pipeline

A possible improvement would be to simplify the code better. For example, I am using `math.atan2()` to sort the slope of lines, and use `math.tan` again to draw the line. That's because I thought the `math.atan2()` will help me to sort the slopes better. But I can also simply use `(y2-y1)/(x2-x1)`. And 

Another potential improvement could be to add some kinds of filter to make the lines look more stale between frames, instead of being "jumpy".

Also I want to make it work better running in Raspberry pi to detect lanes realtime in my car, which can be use to test my codes for coming projects.
