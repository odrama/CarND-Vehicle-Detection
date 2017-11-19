## Writeup Template
### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

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
[image1]: ./examples/car_not_car.png
[image2]: ./examples/HOG_example.jpg
[image3]: ./examples/sliding_windows.jpg
[image4]: ./examples/sliding_window.jpg
[image5]: ./examples/bboxes_and_heat.png
[image6]: ./examples/labels_map.png
[image7]: ./examples/output_bboxes.png
[image8]: ./output_images/download(1).png
[image9]: ./output_images/download(2).png
[image10]: ./output_images/download(3).png
[image11]: ./output_images/download(4).png
[image12]: ./output_images/download(5).png
[image13]: ./output_images/download(6).png
[video1]: ./project_output.mp4


## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the function named `get_hog_features()` in the Jupyter notebook `P5.ipynb`

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Using the `skimage.hog()` function, and playing around with its parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`), i was able to extract HOG features from the vehicle and non-vehicle dataset provided in the lectures.

Here is an example using the `YCrCb` color space and HOG parameters of `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:


![alt text][image2]

#### 2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters and `orientations=9`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)` and using the HOG features of all three channels of the `YCrCb` space worked best for me.

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using a combination of color features, spatial features and HOG features for all three channels. I then instantiated a linear SVM and trained it. On the smaller dataset, i experimented with GridSearchCV for a parameter optimization, which always achieved a higher accuracy, but even then it took too long, so i setteled for the Linear SVM for the bigger data, so i can experiment faster. `Code present one cell after function definitions`

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I understood the lectures, then i re-implemented the `slide_window` function myself in order to get a better grasp of it. I used 70% overlap so that i would have even more areas to search for cars in, and the other parameters were similar to the lecture's. Although i do not fully understand the window subtracted from the total count of windows in ` nx_windows = np.int((xspan - x_buffer) / x_step)`, i mean why subtract a buffer.

![alt text][image3]

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Using 3-channel hog features of YCrCb color space, spatial, and color features (Same as in training) the classifier was able to
recognize the cars. The example images are shown down in question 2 in Video Implementation
![alt text][image4]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_output.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

Using the heatmap concept introduced in the lectures, it was possible to eliminate most of false positives by thresholding for a minimum number of detections. In addition `scipy.ndimage.measurements.label()` was used to identify vehicles based on the output of the thresholded heatmap. Bounding boxes were then superimposed over each vehicle.

### Here are six frames and their corresponding heatmaps and summarized bounding boxes:

![alt text][image8]
![alt text][image9]
![alt text][image10]
![alt text][image11]
![alt text][image12]
![alt text][image13]

### Here is the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames:
![alt text][image6]

### Here the resulting bounding boxes are drawn onto the last frame in the series:
![alt text][image7]



---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

This project was relatively easier than the others, as it was explained for the most part through the lectures and i implemnted the functions alongside, so when i arrived at the project, i had already 90% of the functions ready.
Based on what i read on the forum, some people trained a DNN to recognize the cars, and they said it generally performed better. Another approach is use `GridSearchCV` to optimize the parameters of a SVM for better classification. Tuning the parameters of the already implemented code for less false positives. 

