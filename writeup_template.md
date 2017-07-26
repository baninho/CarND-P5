##Writeup Template
###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./1_hog_car.png
[image2]: ./2_hog_not_car.png
[image3]: ./3_windows.png
[image4]: ./4_detections.png
[image5]: ./5_boxes.png
[video1]: ./project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the code cell labeled HOG Feature Extraction Interface of the IPython notebook.

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

I visualized the HOG representation of a car and non-car image each in the code cells labeled 'HOG Visualization of Car Image' and 'HOG Visualization of Non-Car Image' respectively.

![alt text][image1]
![alt text][image2]

####2. Explain how you settled on your final choice of HOG parameters.

I experimented with some parameters for HOG feature extraction and ended up with the parameters found in the code cell labeled 'Settings and Parameter Definition'. I selected these by training the classifier using different parameters and looked for the best validation result. 



####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using the training data acquired in the code cell labeled 'Training Data Preparation'. The code can be found in the cell labeled 'Classifier Training'. In addition to HOG feautres I used both spatial and color histogram features because I found the most accurate results this way.

###Sliding Window Search

####1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I tried different sizes and overlap margins of windows and decided to use three window sizes of 100, 128 and 160 pixels. I arrived at these values after some test on the provided test images. The distribution of the search windows is visualized in the image below.

![alt text][image3]

####2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

The described parameters produced acceptable detection results, as in the image below. A measure taken to improvae performance was the selection of a larger third window size to reduce window numbers, but there is certainly a lot of room for improvement.

![alt text][image4]
---

### Video Implementation

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video.mp4)


####2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

For averaging I used the heat map integratively over the whole sequence, multiplying it with 0.6 after each frame and subtracting half its minimum value after that.

Below is an image of the bounding boxes drawn by my pipeline function onto one of the provided test images.

![alt text][image5]




---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Skipping and wobbling of the bounding boxes can be tackled by using more sophisticated techniques of averaging over multiple frames. A Vehicle object can be used to track various properties of vehicle detections, such as its trajectory and when it was last detected. 

