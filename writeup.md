#**Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./intermediary/gray.jpg "Gray"
[image3]: ./intermediary/equalized.jpg "Equalized"
[image4]: ./intermediary/yellow_mask.jpg "Yellow"
[image5]: ./intermediary/white_yellow_mask.jpg "White_Yellow"
[image6]: ./intermediary/edges.jpg "Edges"
[image7]: ./intermediary/edges_masked.jpg "Edges_masked"
[image8]: ./intermediary/hough_lines.jpg "Hough_lines"
[image9]: ./test_images/out-whiteCarLaneSwitch.jpg "Out"

---

### Reflection

###1. Pipeline overview:

The pipeline consists of the following steps:

####1. Convert the RGB image to grayscale:
![alt text][image2]

####2. Perform a histogram normalization over the grayscale image in order to improve the contrast of the image; this is helpful in discolored regions of the road:
![alt text][image3]

####3. Convert the RGB image to HSV in order to boost the yellow areas of the image

####4. Compute a yellor mask from the HSV image:
![alt text][image4]

####5. Similarly, compute a white mask from the HSV image

####6. Combine the two masks, to create a white & yellow mask
![alt text][image5]

####7. Apply the white & yellow mask to the equalized grayscale image

####8. Apply a Gaussian smoothing

####9. Perform Canny edge detection:
![alt text][image6]

####10. Define a region of interest and mask the edges outside this region:
![alt text][image7]

####11. Compute the Hough lines, consolidate and extrapolate them:
![alt text][image8]

####12. Apply the lines to the original image
![alt text][image9]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function as follows: 

####1. First, I considered the segments corresponding to the left lane to be those whose slope was < 0.5, and the ones corresponding to the right lane to be those whose slope was > 0.5

####2. For each of the two sets of segments, I computed the average segment, its slope and intercept;

####3. Having the slope and the intercept of the average segment, I try to expand it between the bottom of the image and the top of the region of interest

####4. If the two segments' intersection point lies inside the region of interest (as is the case of the last video), meaning that the selected region of interest is too big, I expand them less (between the bottom of the image and a point inside the region of interest), so that they don't intersect


###2. Potential shortcomings with the current pipeline

One potential shortcoming would be what would happen when a yellow or white car (or obstacle) would be placed in front of the car, inside the region of interest; in this case, the curret pipeline would detect its edges, which will influence the computation of the average segments, moving them from the correct track.

Another shortcoming could be the existance of lanes of different colors, other than white or yellow; in this case, the current pipeline will fail to detect them

Another shortcoming could be the absence of lanes on the road; a pipeline should detect the road and virtually draw left and right lanes if they aren't phisically marked on the road

###3. Possible improvements of the pipeline

A possible improvement would be to perform some sort of obstacle detection and mask out the obstacles laying in front of the car, so that their edges would not be detected

Another improvement would be to make the detected lines follow the direction of the road (instead of being straight lines in front of the car)
