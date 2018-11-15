# Project 1 - Finding Lane Lines

### Writeup

---

**```Josh Schweigert     |     11/14/18     |     jjschweigert.personal@gmail.com```**

---

### Project Goals
* The line identificataion pipeline takes road images from a video and returns an annotated video.

	[Final Goal](examples/P1_example.mp4)


* Pipeline is able to identify left and right lane lines with either line segments or solid lines using the helper functions or other code.

	![Intermediate Image Goal](examples/line-segments-example.jpg)


* Lane boundaries are fully extended via filtering/averaging/extrapolating

	![Final Image Goal](examples/laneLines_thirdPass.jpg)

---

### Lesson Concepts Practiced
During lesson 3 we learned many concepts related to images and these concepts were then applied in this project to a video, or a stream of images. These concepts included,

* #### Color Selection
* #### Region Masking
* #### Color Region
* #### Canny Edge Detection
* #### Hough Transform

### Project Reflection

#### Current Pipeline

To achieve the intermediate goal of finding the solid and dashed lane lines I used a 6 step workflow outlined below:
1. Convert image to grayscale
2. Apply Gaussian smoothing to filter out noise
3. Use Canny edge detection to find edges
4. Apply a region of interest to focus on the current lane
5. Apply a hough transform to find all line segments
6. Overlay the hough transform line segments on the original image

To achieve the final goal of an extrapolated line for the left and right lanes I modified the draw lines function
which is called by the hough_lines helper. This process involved the following steps:

1. Cycle through each line segment and find the center point and slope
    * To determine if the line segment is left or right we look at the sign of the slope, positive for right and negative         for left
2. Find the average center and slope for each side
3. We now have the necessary parameters to plug into the standard equation for a line: (y - y') = m(x - x')
    * Since we well have a slope and a point (x', y') we need to determine which to solve for, x or y.
      For my particular area of interest the two points furthest away in the picture but closest to the x axis both had the       same y value and the two points closest or furthest down both had the same y value. So I used these Y values of the         four points and our parameters to calculate the new x value for each point.
4. After the new points were found I used cv2.line to draw a line segment for the two points on the left lane and a segment    for the two points on the right lane.


#### Potential Limitations

After running the pipeline and seeing the output there is much work to be done. Some potential limitations are the shape used for the area of interest. This effects the domain where in the lane lines may extend. Another is the method of extrapolation. For the method I used, I did not get a stable output, either the lines crossed over each other or there was flickering while playing the video.

Another limitation might be the number of line segments that are found for each lane. If not many are found then the result may not be that great but more data means more accuracy.


#### Future Development

To improve this project in the future some modifications would be using the polyfit function in the numpy module with the goal being a much smoother and more accurate output. Another would be looking at my area of interest and seeing if tuning that will help with results. Last I would work on the flickering issue and ways to alleviate the displacement of lines throughout the video images as they are processed.