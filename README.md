# CarND-Vehcile-Detection
## UCity Project for Vehicle Detection

* Find the original instructions for this project in Udacity's repo
---

The Goals / Steps of this project

*Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled traning set of images and train a Linear SVM Calssifier
*Implement Sliding-window technique and use your trained classifier to search for vehicles in images
*Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full projecy_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
*Estimate a bounding box for vehciles detected

## Histogram of Oriented Gradients (HOG)
#### 1. Explain how you extracted HOG features from the training images
Started by loading all the vehcile and non-vehcile images (randomly show few samples ) under training_ds.

REF to code section "Visualizing few Training samples" to view images plotted
Defined a function called "extract_hog_features" to extract HOG features of each input image. Accepts list of image paths and HOG parameters and produces a flattened array of HOG features for each image in list.

REF to Sections "Method to transform input image to HOG (Histogram of Oriented Gradinets) image" and "Visulaizing HOG image" which has the implemenation of HOG feature extract method and visulalizing HOG feature o/p for Vehicle and non-vehcile sample input.
Next step is to extract features for Input datasets and Combine, define labels vecotr, shuffle and split train and test dataset. By defining HOG feature extraction and extract features for the entire dataset. These features sets are combined and a label vector is defined ( '1' for Vehciles (Car) and '0' for non-cars).

#### 2. Selecting HOG parameters
The features and labesl are then shuffled and split into traning and test sets in preparation to be fed to linear Support Vector Machine (SVM) classifier. Chossen HOG parameters based on performance and accuracy of the SVM classifier predicting cars and non-cars accurately and also quickly.

#### 3. Describe how you trained a classifier using your selected HOG features
In the section titled "Train a Classifier" I trained a linear SVM with the default classifier parameters and using HOG features alone and was able to achieve a test accuracy of 98.17%.

## Sliding Window Search
#### 1. How you implemented a sliding window search. What scales to search and how much to overlap windows?
In this "Detecting Cars Using trained Classifier" section I adapted method called "Detect_cars" from the materials explained in the class. Method combines HOG feature extraction with a sliding window search. HOG features are extracted for the entire image and then these full-image features are subsamples according to the size of the window and then fed to the classifier.

The method performs the classifier prediction on the HOG features for each window regions and returns a list of rectangle objects correspoding to the windows that genearted a True predction ("Car")

Ref to section "Drawing bounding box on detected objects (Car) in a given image" where Boudning box is draw on sample test image where Car is being detected.

#### 2. Show some examples of test images to demonstrate how the pipeline is wokring. What did you do to optimze the perforamnce of your classifier?
Ref to the section "Building pipeline" which takes test_images as inputs, on which we extract HOG features and pass it to SVM predictor to predict the label (car or non-car).

## Video Implementation
#### 1. Provide a link to your final video output. Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Ref to section "Run Video Through pipeline" which takes 'test_video.mp4' as input and runs it through "detect_car" and shows the output on the plyaer with bounding box draw on detected cars.
#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.
Ref to section "Class to store Vehcile Detection" where past 15 frame output history is combined and added to the heatmap and the threshold for the heatmap is set to '1+len(det.prev_rects)/2. This value was found to perform best emprically.

## Discussion
### 1. Issues faced.
Seen issues seeting ystart and ystop positions duirng sliding window selection
Faced issues which "Expecting 2D array but seen only 1D". Used reshape(1,-1) to fix this
Acheving detection accuracy was a big challenge - Maximing window overlap during sliding helped to resolve that
