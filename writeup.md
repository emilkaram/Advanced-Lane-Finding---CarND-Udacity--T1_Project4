# Advanced lane finding project
### This project to find lane lines using advanced computer vision techniques.
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
### Camera Calibration and Distortion correction
I computed the camera matrix and distortion coefficients by:
a.	Find corners on chessborad calibration images:

Set arrays to store object points(Distorted points) and image points (undistorted image) from all the images
objpoints = [] 
imgpoints = []
loop through the claibration images (after converting to gray scale) and find corner and display corners on the image using cv2.findChessboardCorners and draw the corners using cv2.drawChessboardCorners

b.	Camera calibarion (find camera matrix and distortion coefficient)
Using the object points and image points mapping I can compute camera matrix and distortion coefficient using cv2.calibrateCamera

### Pipeline (single images)

## Distortion correction
I applied camera matrix and distortion coefficient to the test image using the cv2.undistort() function and obtained this result:


  Here an examples of a distortion corrected calibration image.



## Gradianet Threshold and Color Threshold
I tested color threshold and gradient threshold on test images to decside the best comniation to use in my pipline

## Color Threshold

Gray and Gray binary:


RGB

R and R binary

HLS

S and S binary



## Gradianet Threshold:
 
Absolute Sobel x gradient
Absolute Sobel y gradient

Magnitude of Gradient
Direction of Gradient

Combined Gradients:
 


## Combine Color and Gradient thresholds

Finaly I decide to combine color thresholds and gradients , I used S binary and R binary , Sobel X binary , Sobel y binary , Magnitude of Gradient binary ,
Direction of Gradient binary 

I tried different mathematical comnination using “AND”  “ OR” function 
And here the final combination :
combined_binary[(S_binary==1)&(R_binary==1)|((x_binary_output==1)&(y_binary_output==1))|((mag_binary_output==1)&(dir_binary_output==1))]=1

 here is example of the Combine Color and Gradient thresholds applied to test image




### Perspective Transform:

I defined 4 points to be used as source point in perspective transform
Then defined 4 points to be used as destination point in perspective transform

 



I calculated the perspective matrix M to warp an image using 
        M=cv2.getPerspectiveTransform(src,dst)

and also the inverse matrix invM  
 Minv=cv2.getPerspectiveTransform(dst,src)
Calsuatlted wrap and unrap fuction using 
warped = cv2.warpPerspective(img,M,img_size,flags = cv2.INTER_LINEAR)
        unwarped = cv2.warpPerspective(img,Minv,img_size,flags = cv2.INTER_LINEAR)

I verified that my perspective transform was working as expected by drawing the src and dst points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

here examples for warped fuction alliped to test images




and here all the steps applied to test images (camera calibration ,distcotion correction , color theroshld , gradient and warped)


### Final step is to Locate the lane lines and fit polynomial (Sliding window)
l saved the warped binary image in "binary_warped" and take a histogram of the bottom half of the image 
I found the peak of the left and right halves of the histogram
 These will be the starting point for the left and right lines

I choose the number of sliding windows = 9 and Set height of windows
Identify the x and y positions of all nonzero pixels in the image
I set the width of the windows +/- margin = 100 and Set minimum number of pixels found to recenter window=50
Created a loop to Step through the windows one by one
Concatenate the arrays of indices , Extract left and right line pixel positions , Fit a second order polynomial to each 

And here the results


# Now I have a new warped binary image 
# from the next frame of video (also called "binary_warped")
# It's now much easier to find line pixels!
And here the results





 
 
 
   :
 
________________________________________
### Pipeline (video) / disscussion
I used 2 methods:
First method to use sliding window through the etire video (process_image function )
Second method use the sliding window to (process_image2 function) only to find lines on first frame then lines can be estimated this method works ok but with little wobble in couple of spots , to make it perfext I try to recrate lines using sliding window every 50 or 100 frames and results was perfrect 
Both methods pipeline performed perfectly well on the entire project video  
Here's a link to my video result
________________________________________
 
 

