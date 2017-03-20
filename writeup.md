#**Finding Lane Lines on the Road** 

##Writeup

I will descript my onion about how handle the image for finding lane lines on the road in this file.


###Pipeline Method

I get gray image from the native image, and apply Gaussian blur and the Canny edge detection algorithm for the gray image. I get the output image below:

[canny_image]: ./test_images_output/solidWhiteRight_Canny.jpg "CannyIamge"
![alt text][canny_image]

then I define a four sided polygon to mask the image after appling the canny edge detection algorithm. And I get the output image below:

[region_image]: ./test_images_output/solidWhiteRight_Region.jpg "RegionIamge"
![alt text][region_image]

Here I use the Hough transform with it and get the detected lines. I split the detected lines to different sides and draw two final lines. 

In order to draw a single line for each lane, I implemente another function for drawing lines: draw_lines_by_middel_func().

In this function I assumption that the points in the left side of image belong to the left lines, and the right is opposite. So I use the middle width of image as the value, and I handle the two different conditions:

* if the x value of piont is larger than the defined value, we think the point is in right line;
* if the x value of piont is smaller than the defined value, we think the point is in left line;
* otherwise we don't use the piont.

Importantly, I calculate the ratio of detected line and assupmtion the ratio of different side in a reasonable range (here ratio in left line is between -inf ~ 0 and ratio in right line is between 0 ~ +inf).

At last, we draw the left line and right line with the minimum value. So I get the output image below:

[hough_image]: ./test_images_output/solidWhiteRight_Hough.jpg "RegionIamge"
![alt text][hough_image]

Finally, I get the output image as below:

[result_image]: ./test_images_output/solidWhiteRight_Out.jpg "RegionIamge"
![alt text][result_image]



## Reflection

###1. draw_lines() method

My draw_lines() function handle the lines as below:

1. divied the lines to different parts: left side and right side
	* if x < width/2 and the ratio of the line < 0, then is in left side
    * if x > width/2 and the ratio of the line > 0, then is in left side
2. calculate the ratio and the intercept of the different line
3. calculate the average of the ratio and the intercept in both side
4. get the two points according to the y_min and y_max for both side
5. draw the lines


###2. Potential shortcomings


One potential shortcoming would be that it can't handle the lane line of bend. I assume that the lane line in mask region will be straight, So it will be incorrect in bend.

Another shortcoming could be that the draw line function is not perfect. There are a lot of assumptions in function.


###3. Possible improvements

I think the biggest improvement is that changing the draw line function. Maybe we can draw lines instead of a single line. If we do it, the function will can handle the lane lines of bend.