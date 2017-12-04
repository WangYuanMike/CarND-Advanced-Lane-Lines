**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./examples/undistort_output.png "Undistorted"
[image2]: ./test_images/test1.jpg "Road Transformed"
[image3]: ./examples/binary_combo_example.jpg "Binary Example"
[image4]: ./examples/warped_straight_lines.jpg "Warp Example"
[image5]: ./examples/color_fit_lines.jpg "Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the jupyter notebook located in [advanced_lane_line.ipynb](https://github.com/WangYuanMike/CarND-Advanced-Lane-Lines/blob/master/advanced_lane_line.ipynb) 

I use the way stated in the lecture, just changed the corner numbers from (8, 6) to (9, 6).


### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

See output of jupyter notebook cell No. 2.

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image. See detail and example image in cell No. 3.

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warp()`, which appears in cell No. 4 in the jupyter notebook. The `warp()` function takes as inputs an image (`img`), as well as source (`src`). `dst` is a variable in the function.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32([[195, 719], [593, 450], [688, 450], [1118, 719]]))
dst = np.float32([[src[0,0]+offset, src[0,1]], 
                  [src[0,0]+offset, 0], 
                  [src[3,0]-offset, 0], 
                  [src[3,0]-offset, src[3,1]]])
offset = 150
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 195, 719      | 345, 719      | 
| 593, 450      | 345, 0        |
| 688, 450      | 968, 0        |
| 1118, 719     | 968, 719      |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

see the example image in output of cell No. 4 in the jupyter notebook.

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

This is done in cell No. 5 and cell No. 6 of the jupyter notebook. In the function sliding_window_fit(), it takes a binary warped image and use the sliding window way to fit the left an right lanes. This is main fit function I used in the pipeline to find the lane lines. The other function margin_fit() is implemented but not used in the pipeline. See example output images in cell No. 5 of the jupyter notebook.

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in cell No. 7 of the jupyter notebook. I used the way taught in the lecture. See example output images in cell No. 7 of the jupyter notebook.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

See the line warp example in cell No. 8 of the jupyter notebook, and the example line warp image with left and right curverad and center offset text in cell No. 9 of the jupyter notebook.

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](https://github.com/WangYuanMike/CarND-Advanced-Lane-Lines/blob/master/P4_video_final.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Issues with pipeline:
* easy to fail when the road surface has different colors, which makes the pipeline hard to find the accurate lane position
* sometimes the pipeline could be impacted by the cars coming from right lane

Possible approach to solve the issue:
* Stablize the lane lines by averaging lanes in previous images

Questions:
* Could you please give some example of the tricks(e.g. sanity check, look-ahead filter, reset) mentioned in [this lecture](https://classroom.udacity.com/nanodegrees/nd013/parts/fbf77062-5703-404e-b60c-95b78b2f3f9e/modules/2b62a1c3-e151-4a0e-b6b6-e424fa46ceab/lessons/40ec78ee-fb7c-4b53-94a8-028c5c60b858/concepts/7ee45090-7366-424b-885b-e5d38210958f)?
