# **Finding Lane Lines on the Road** 

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I applied Gaussian smoothing to reduce image noise.
Next I applied the Canny transformation to detect edges in the original image and greatly reduce the amount of data to be processed.
After the Canny transform I applied another transformation, the Hough transform, this is used to find specific shapes, in this case lines, in an image by a voting procedure. The voting is done via the hough space in which local maxima is found. The Hough transform allows us, using a set of parameters, to group together points in the image that make up a shape, or in this case a line.
Finally using the lines detected earlier and defining a region of interest I was able to detect, and draw in red, the lane lines in the original image.
This was then applied to each frame, or image, in the video.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first calculating the `m` and `b` in the line equation. Then using the fact that a positive `m` indicates a positive slope and therefor identifies the right lane I proceeded to find 2 points to draw the line, one point close to the centre of the image, and another close to its lower edge.
For the first point I defined that the `y` coordenate must have the same value as the higher (lowest in value) point in the region of interest, I called it `y_t`, then, by using the equation of the line, `y = mx + b`, and the values for `m` and `b` calculated previously I calculated the value of `x`, in this case `x1 = (y_t - b ) / m`.
The same formula was applied for the second point, by fixing the `y` to the lower edge of the image (highest value for y), I called it `y_m` for maximum. Then `x2 = (y_m - b ) / m`.
The same was applied to the left lane, in this case by swaping the order of the points given the negative slope.


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when a sharp bend appeared in front of the car. 
This pipeline was designed and tested on highway images and videos with very small bends and would probably perform subpar in case of a sharp bend.

Another shortcoming could be the case where the lines are worn out or simply not existent, like in some rural roads. Again the pipeline was not tested in these circumstances and would most likely struggle to identify lanes, or the edges of the road, in that scenario.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to overlay a transparent colour in between the lane lines, giving a clearer indication of the area the car should occupy.

Another potential improvement could be to use a HLS space together with the quadratic equation to draw curves instead of lines, fixing the first shortcoming mentioned above, this would be a better approximation of a real world scenario.

To correct for the second shortcoming a bigger data set would be needed, one that includes images and videos from poorer maintained roads. Then we could further develop the pipeline to be able to detect the edges of the lane/road based a wider set of rules that matched the bigger data set.
