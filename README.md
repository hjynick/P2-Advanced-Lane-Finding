## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


In this project, your goal is to write a software pipeline to identify the lane boundaries in a video, but the main output or product we want you to create is a detailed writeup of the project.  Check out the [writeup template](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) for this project and use it as a starting point for creating your own writeup.  



The Project
---

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

The images for camera calibration are stored in the folder called `camera_cal`.  The images in `test_images` are for testing your pipeline on single frames.  If you want to extract more test images from the videos, you can simply use an image writing method like `cv2.imwrite()`, i.e., you can read the video in frame by frame as usual, and for frames you want to save for later you can write to an image file.  

To help the reviewer examine your work, please save examples of the output from each stage of your pipeline in the folder called `output_images`, and include a description in your writeup for the project of what each image shows.    The video called `project_video.mp4` is the video your pipeline should work well on.  

The `challenge_video.mp4` video is an extra (and optional) challenge for you if you want to test your pipeline under somewhat trickier conditions.  The `harder_challenge.mp4` video is another optional challenge and is brutal!

If you're feeling ambitious (again, totally optional though), don't stop there!  We encourage you to go out and take video of your own, calibrate your camera and show us how you would implement this project from scratch!

Generally we need to build a pipeline in this project and use it to process the images and the videos. the steps of this pipeline i can show you guy as followed:

### first, we've got some chessboard pictures to calibrate the camera image

In this step, i apply some openCV functions on the chessboard picture so that i can get the calibration matrix.

### second, i use perspective transform so that i can get the transformed lane lines

in this step, i got the transform matrix.

### third, i use the colorspace threhold to extract the pure lane lines of the transformed image

I've combined several filter such as sobel filter and the hls filter to get the best results.

### forth, i use the sliding windows algorithms to calculate the active pixel and got the polylines

Using the ploylines we can get the curvature and the radios of the curve.

### finaly, i combined all the steps to build a pipeline and use it to process the image and the videos.



# Camera Calibration

* Have the camera matrix and distortion coefficients been computed correctly and checked on one of the calibration images as a test?

The code for this step is contained in the first code cell of the IPython notebook (or in lines # through # of the
file called ···some_file.py ···).
I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the
world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are
the same for each calibration image. Thus, objp is just a replicated array of coordinates, and objpoints
will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.
imgpoints will be appended with the (x, y) pixel position of each of the corners in the image plane with
each successful chessboard detection.

I then used the output objpoints and imgpoints to compute the camera calibration and distortion
coefficients using the ···cv2.calibrateCamera() ···function. I applied this distortion correction to the test
image using the ···cv2.undistort()··· function and obtained this result:

  ![undistort]('../CarND-Advanced-Lane-Lines/output_images/undis_output.png')

# Pipeline (single images)
### 1. Has the distortion correction been correctly applied to each image?
* To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this
one:
  ![image]('..\CarND-Advanced-Lane-Lines\output_images\cali_output.png')
  

### 2. Has a binary image been created using color transforms, gradients or other methods?

* Oooh, binary image... you mean like this one? (note: this is not from one of the test images)

  ![image]('../CarND-Advanced-Lane-Lines/output_images/binary_output.png')
  
### 3. Describe performence of a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warper()`, which appears in lines 1 through 8 in the file `example.py` (output_images/examples/example.py) (or, for example, in the 3rd code cell of the IPython notebook).  The `warper()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32(
    [[(img_size[0] / 2) - 55, img_size[1] / 2 + 100],
    [((img_size[0] / 6) - 10), img_size[1]],
    [(img_size[0] * 5 / 6) + 60, img_size[1]],
    [(img_size[0] / 2 + 55), img_size[1] / 2 + 100]])
dst = np.float32(
    [[(img_size[0] / 4), 0],
    [(img_size[0] / 4), img_size[1]],
    [(img_size[0] * 3 / 4), img_size[1]],
    [(img_size[0] * 3 / 4), 0]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 585, 460      | 320, 0        | 
| 203, 720      | 320, 720      |
| 1127, 720     | 960, 720      |
| 695, 460      | 960, 0        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![image]('../CarND-Advanced-Lane-Lines/output_images/perspective_output.png')

### 4. dentified lane-line pixels and fit their positions with a polynomial.

I defined the fit_poly to fit the lane lines with a polyline.



### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.



### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![image]('../CarND-Advanced-Lane-Lines/output_images/perspective_output.png')

### Pipeline (video)

#### 1. Provide a link to final video output.  

Here's a [../CarND-Advanced-Lane-Lines/result.mp4](./project_video.mp4)

---

### Discussion

#### 1 the fit output

in the output videos, the right lane looks well but it looks like that the algorithms did not fit the left lane very well. it is probably because that the colorspace threhold filter didn't works very well. a friend of mein has applied the lab colorspace with the l and a channel to extract the lane line and it works well and can resist the sunshine and the shadow. so i would like to try this laterly.

#### 2 the lane class

so i didn't really use the object oriented programing because the attribute of the lane line is a little few. but i've tried this and it looks good.

#### 3 sliding window 

using the sliding window algorithms to extract the lane pixel works well but it takes much time. so i will develop a new algorithms laterly.

i've learn lots of things from this project and i should always improve my program skill.