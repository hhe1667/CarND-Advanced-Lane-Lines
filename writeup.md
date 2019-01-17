
# Advanced Lane Finding Project

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



### Camera Calibration

I did the camera calibration in an IPython notebook [camera_calibration.ipynb](camera_calibration.ipynb).  

First, I create a grid of chessboard corners on the z=0 plane.
Second, I detect the chessboard corners in each of the calibration images using `cv2.findChessboardCorners()`.
Then, I used the chessboard corners and detected image points to calibrate the camera using `cv2.calibrateCamera()`.
The output is a calibration matrix and distortion coefficients. I store the output to a pickle file.

Below is an example of undistortion. More examples can be found in [camera_calibration.ipynb](camera_calibration.ipynb).

![calibration1.jpg](camera_cal/calibration1.jpg)
![Undistorted image](output_images/calibration1_undistorted.jpg)


### Pipeline (single images)
The pipeline is implemented in IPyhton notebook [pipeline.ipynb](pipeline.ipynb).
`Pipeline` is the main class and `process_image()` is the main function.

#### 1. Undistort the raw image.
The first step is to undistort the raw image, which is implemented using
`cv2.undistort()` with the calibration matrix and distortion coefficients.
Below is an example undistorted image:

![test1_undistorted.jpg](output_images/test1_undistorted.jpg)

#### 2. Color and gradient thresholding.

I used a combination of color and gradient thresholds in `threshold_image()`:
1. RGB threshold [200, 200, 10].
2. S-channel threshold [150, 200] in the HLS color space.
3. Gradient threshold: convert to grayscale; Gaussian blurring; Sobel filtering with magnitude threshold [40, 100].
Below is an example thresholded image (Red for RGB; Green for S-channel; Blue for gradient):

![test1_thresholded.jpg](output_images/test1_thresholded.jpg)

#### 3. Perspective transform.
I pre-computed the perspective transform matrix and inverse matrix in
`get_perspective_transform()`. The source and destination points were manually
picked from an example image.
Then, in the function `process_image()` I perform the perspective transform.
Below is an example warped image:

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![test2_warped](output_images/test3_warped.jpg)

#### 4. Find lane-line pixels and and fit their positions with a polynomial.

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
