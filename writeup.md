#**Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./output_images/output_edges.jpg "Edges"
[image3]: ./output_images/output_masked.jpg "Masked Image"
[image4]: ./output_images/output_lines.jpg "Lane Lines"
[image5]: ./output_images/outputwhiteCarLaneSwitch.jpg "Blended Image"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

* My pipeline consisted of 5 steps. First, I converted the images to grayscale using the Open CV built in gray scale function. With the gray scaled image I applied a Gaussian smoothing function using a kernal size of 5. 
![alt text][image1]

* Next, using the canny edge detection built in function in Open CV. I applied a 2:1 (80-160) high to low threshold ratio in order to achieve the resolution desired. 

![alt text][image2]

* With the canny edge detection image, I then created a mask for the image in order to only process lines in the appropriate location. Using a simple four sided polygon, with vertices based on the edges of the image, as well as set points that are always needed to be included in lane finding algorithm. 

![alt text][image3]

* Using only the data only from the masked part of the image, I then applied a Hough Transform to the lines in the image. This limits the number of line segments included, based on segment length, pixel distance etc.

* After applying the Hough Transform, the number of line segments included in the image is reduced. Noticing the left lane line has a negative slope and the right lane line has a positive slope, I decided to separate the line segments based on their slope. I applied several filters for each line, including a slope range which included all potential lane line slopes, as well as a location filter, which only included lines on the left of center in the left line extrapolation, and lines right of center in the right lane line extrapolation. Using a polyfit algorithm which extrapolated the lane line based on each point included in the filtered line segments. Applying this algorithm produces the following lines. 

![alt text][image4]

* The found lane lines were then blended with the actual image to produce the final image with the visualized found lane lines. 

![alt text][image5]

###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when there are shadows introduced into the lanes. I have not been able to produce a successful challenge video, mostly due to the shadows on the left side of the image. Additionally, if there was a car introduced directly in front of our car, there might be issues with finding the lane lines in front of leading car. 


###3. Suggest possible improvements to your pipeline

A possible improvement would be to develop a method for producing a mask based on the current image, not based on arbitrary dimensions. This would potentially help account for the leading car issue, previously discussed. Additionally, developing an algorithm to determine the points to be included in the lane line segments based on a range of slopes that are most densely populated might allow for more accurate lane detection. 

