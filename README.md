# Advanced_Lane_Finding
This project identifies both yellow and white Lane Lines in different conditions like roads with shadows and color gradient.

## Objective :

The main of this Advanced Lane Finding project is to,

* Identify both yellow and white Lane Lines in different conditions like roads with shadows and color gradient.

* Rejection of Shadows and, variation in lane and environment colors shall be done using HLS filter and Sobel Operator (Absolute, Magnitude and Direction filters).

* The image which was processed to identify the lanes shall be free from distortion. Distortion shall be avoided by using the calibration matrix obtained from calibrating the camera with the provided Chess board images.

* The Lane pixels shall be identified using the image in Bird’s Eye view (Top View) and not from normal view. This rectified image shall be obtained by using Perspective transformation on the original image with predefined Source and Destination points.

* Identify the curvature of the Lane (in meters) and vehicle position with respect to center. These details shall be displayed in the final image.

* Merge the identified Lane boundaries (with color) into the original image.

## Pipeline:

1. Identify **Chess board corners**.
2. Calibrate Camera by identifying **Camera matrix and distortion coefficients**.
3. **Undistort** the image.
4. Choose **Color Channels** which needs to be activated for proper identification of lanes in all conditions.
5. Form a binary threshold values from using **Gradient thresholds** in the Direction of X and Directional Gradient.
6. **Mask** the Lane region.
7. Transform the Binary Image into Warped image (Bird’s Eye view) using **Perspective Transform**.
8. Fit **Polynomial Curve** by Identifying the Lane Pixels using **Sliding Window method**.
9. In order to reduce the processing of the lane pixel detection, use the **last searched** and identified pixel data.
10. Find the **curvature of lane**.
11. Calculate the **center of Vehicle** with respect to the Lane center.
12. Display the **identified lane image** over the **Actual image** along with proper text to display curvature and vehicle center.

## Solution


**1. Camera Calibration:**

The given chessboard images was processed to identify the chessboard corners using the function **IdentifyChessBoardCorners()**. Then the Camera matrix and the Distortion coefficient was calculated in the function **CalibrateCamera()**.


**2. Distortion Correction:**

The identified camera matrix and distortion coefficients was used for distortion correction of any further image which will be processed for lane detection. This steps was handled in the function **DistortionCorrec()**.

**3. Color Transform and gradients:**

The L and S channels are used by applying channel filter and limiting values wit threshold. This was handled in the function **GetColorTransform()**. Gradient threshold was applied in the direction of X and using Directional gradient in the function **abs_sobel_thresh()**, **dir_threshold()**. Finally, all these color and gradient thresholds are merged in a way to eliminate unwanted details like shadows, color gradient in the road. A mask for a specified region of lane was applied to filter only lane data.

**4. Perspective Transform:**

Transform the obtained thresholded image to a top view image (Bird’s eye view). This can be done by Perspective Transformation of images using source and destination points which was implemented in the function **PerspectiveTransfrm()**. The transformed lanes are always parallel.

**5. Identify Lane pixel and draw polynomial:**

Now identify the lane pixels from the rectified image. This can be done by obtaining the Histogram of the images. Then with the help of Histogram identify 2 max points which will give the lane positions. Start a rectangular area for search of lane pixels, slide it across the image to identify the lane pixels. Based on the observed pixel value find a polynomial which best suits the lane lines. This was done in the function **find_lane_pixels()** and **fit_polynomial()**.


Also, instead of going for a new histogram and sliding window each time, process the lane pixels from the last obtained pixel values by searching around technique. This was obtained using the function **search_around_poly()**.


**6. Radius of Curvature and Center of Vehicle:**
Calculate the Radius of the curvature using the provided formula and the polynomial curve values. Refer **measure_curvature_real()** function for the implementation of calculation of the curvature. The vehicle center is calculated inside the pipeline function itself.

**7. Final Output:**
A polygon was drawn over the actual image to display the identification of lane markings. Also, the radius of curvature and Center of vehicle texts was displayed in the same image.

## Pipeline :

The pipeline was completely implemented in the function **Project_PipeLine()**.
The Image outputs and video outputs are stored in the project folder **output_images**.

## Potential Shortcomings and Challenges:

1. When a vehicle in next lane travels within the region of interest the identified lanes has some small deviations for a negligible amount of time before correcting itself to the actual lane line.
2. When no lane lines are detected, the np.polyfit function will through an error, hence a check was introduced to prevent this issue. When no lanes are detected the pixels from last identified data was used.
3. The source points and destination points in the perspective transform shall be updated based on lanes. As different types of lanes could be available, working with same source points may sometime leads to failure in identify lanes.
