# Advanced lane finding project
### This project to find lane lines using advanced computer vision techniques.

![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/video_output.png)

________________________________________
*Advanced Lane Finding Project
The goals / steps of this project are the following:
*	Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
*	Apply a distortion correction to raw images.
*	Use color transforms, gradients, etc., to create a thresholded binary image.
*	Apply a perspective transform to rectify binary image ("birds-eye view").
*	Detect lane pixels and fit to find the lane boundary.
*	Determine the curvature of the lane and vehicle position with respect to center.
*	Warp the detected lane boundaries back onto the original image.
*	Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

Rubric Points
Here I will consider the rubric points individually and describe how I addressed each point in my implementation.
________________________________________
# Camera Calibration and Distortion correction
I computed the camera matrix and distortion coefficients by finding corners on chessborad calibration images
I set arrays to store object points(Distorted points) and image points (undistorted image) from all the images
objpoints = [] , imgpoints = []
I looped through the claibration images (after converting to gray scale) and find corner and display corners on the image using cv2.findChessboardCorners and draw the corners using cv2.drawChessboardCorners

![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/1.png)


Then I did Camera calibarion (find camera matrix and distortion coefficient)
Using the object points and image points mapping I can compute camera matrix and distortion coefficient using cv2.calibrateCamera

Here an examples of a distortion corrected calibration image

![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/2.png)


# Pipeline (single images)

## Distortion correction
I applied camera matrix and distortion coefficient to the test image using the cv2.undistort() function and obtained this result:


  Here an examples of a distortion corrected image

![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/3.png)

## Gradianet Threshold and Color Threshold
I tested color threshold and gradient threshold on test images to decside the best comniation to use in my pipline


## Color Threshold

Gray and Gray binary:
![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/4.png)

RGB
![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/5.png)

R and R binary
![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/6.png)

HLS
![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/7.png)

S and S binary
![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/8.png)


## Gradianet Threshold:
 
Absolute Sobel x gradient
![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/9.png)

Absolute Sobel y gradient
![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/10.png)

Magnitude of Gradient
![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/11.png)

Direction of Gradient
![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/12.png)

Combined Gradients:
![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/13.png) 

 
## Combine Color and Gradient thresholds
![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/14.png)
![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/15.png)

Finaly I decide to combine color thresholds and gradients , I used S binary and R binary , Sobel X binary , Sobel y binary , Magnitude of Gradient binary ,Direction of Gradient binary 

I tried different mathematical combnination using “AND”  “ OR” function 
And here the final combination :
combined_binary[(S_binary==1)&(R_binary==1)|((x_binary_output==1)&(y_binary_output==1))|((mag_binary_output==1)&(dir_binary_output==1))]=1

 here is example of the Combined Color and Gradient thresholds applied to test image
 
 ![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/16.png)




# Perspective Transform:

I defined 4 points to be used as source point in perspective transform
Then defined 4 points to be used as destination point in perspective transform

![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/17.png)

![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/18.png)


I calculated the perspective matrix M to warp an image using M=cv2.getPerspectiveTransform(src,dst)
and also the inverse matrix Minv using  Minv=cv2.getPerspectiveTransform(dst,src)

I calculated wrap and unrap fuction using 

warped = cv2.warpPerspective(img,M,img_size,flags = cv2.INTER_LINEAR)

unwarped = cv2.warpPerspective(img,Minv,img_size,flags = cv2.INTER_LINEAR)

I verified that my perspective transform was working as expected by drawing the src and dst points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

here examples for warped fuction alliped to test images

 ![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/19.png)
 ![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/20.png)
 ![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/21.png)
 ![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/22.png)
 ![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/23.png)
 ![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/24.png)
 ![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/25.png)
 ![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/26.png)


and here all the steps applied to test images (camera calibration ,distcotion correction , color theroshld , gradient and warped)
 ![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/32.png)
 ![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/27.png)
 

## Final step is to Locate the lane lines and fit polynomial (Sliding window)
l saved the warped binary image in "binary_warped" and take a histogram of the bottom half of the image 
I found the peak of the left and right halves of the histogram
 ![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/29.png)
 ![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/28.png)

These will be the starting point for the left and right lines

I choose the number of sliding windows = 9 and Set height of windows
Identify the x and y positions of all nonzero pixels in the image
I set the width of the windows +/- margin = 100 and Set minimum number of pixels found to recenter window=50
Created a loop to Step through the windows one by one
Concatenate the arrays of indices , Extract left and right line pixel positions , Fit a second order polynomial to each 

And here the results

 ![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/30.png)

# Now I have a new warped binary image 
# from the next frame of video (also called "binary_warped")
# It's now much easier to find line pixels!
And here the results

 ![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/31.png)



 # Pipeline (video) / disscussion
 ![](https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/images/33.png)
I used 2 methods:
First method to use sliding window through the etire video (process_image function )
Second method use the sliding window to (process_image2 function) only to find lines on first frame then lines can be estimated this method works ok but with little wobble in couple of spots , to make it perfext I try to recrate lines using sliding window every 50 or 100 frames and results was perfrect 
Both methods pipeline performed perfectly well on the entire project video  
Here's a link to my video result
https://github.com/emilkaram/Advanced-Lane-Finding-CarND-Udacity-T1_Project4/blob/master/video_output.mp4

 
 

