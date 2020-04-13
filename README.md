# SFND 2D Feature Tracking

<img src="images/keypoints.png" width="820" height="248" />

The idea of the camera course is to build a collision detection system - that's the overall goal for the Final Project. As a preparation for this, I have built the feature tracking part and test various detector / descriptor combinations to see which ones perform best. This mid-term project consists of four parts:

* First, I have focused on loading images, setting up data structures and putting everything into a ring buffer to optimize memory load. 
* Then, I have integrated several keypoint detectors such as HARRIS, FAST, BRISK and SIFT and compare them with regard to number of keypoints and speed. 
* In the next part, I have focused on descriptor extraction and matching using brute force and also the FLANN approach we discussed in the previous lesson. 
* In the last part, once the code framework is complete, I have tested the various algorithms in different combinations and compare them with regard to some performance measures. 

## Dependencies for Running Locally
* cmake >= 2.8
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1 (Linux, Mac), 3.81 (Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* OpenCV >= 4.1
  * This must be compiled from source using the `-D OPENCV_ENABLE_NONFREE=ON` cmake flag for testing the SIFT and SURF detectors.
  * The OpenCV 4.1.0 source code can be found [here](https://github.com/opencv/opencv/tree/4.1.0)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory in the top level directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./2D_feature_tracking`.

## Project Rubrics Explanation:

* MP.1 Data Buffer Optimization: Implement a vector for dataBuffer objects whose size does not exceed a limit (e.g. 2 elements). This can be achieved by pushing in new elements on one end and removing elements on the other end.

The ring databuffer was formed by implementing a logic that would remove the first element from the vector as long as the the length of the vector is greater than 2.

* MP.2 Keypoint Detection : Implement detectors HARRIS, FAST, BRISK, ORB, AKAZE, and SIFT and make them selectable by setting a string accordingly.

The keypoint detectors were implemented in the code matching2D_Student.cpp. It involved using the OpenCV library functions for each of the keypoint detectors. The code for the same has been implemented in lines 180-206.

* MP.3 Keypoint Removal: Remove all keypoints outside of a pre-defined rectangle and only use the keypoints within the rectangle for further processing.

The rect function contain was used to figure out the points that are within the rectangle defined for the preceding car and the ones outside the rectangle were discarded.

* MP.4 Keypoint Descriptors: Implement descriptors BRIEF, ORB, FREAK, AKAZE and SIFT and make them selectable by setting a string accordingly.

The descriptors have been implemented using the openCV library functions. The code for the same is available in the matching2D_Student.cpp from lines 68 to 86.

* MP.5 Descriptor Matching: Implement FLANN matching as well as k-nearest neighbor selection. Both methods must be selectable using the respective strings in the main function.

The FLANN matching and the KNN selection were implemented using their respective OpenCV library functions (matching2D_Student.cpp Lines: 19-27 and 37-50).

* MP.6 Descriptor Distance Ratio: Use the K-Nearest-Neighbor matching to implement the descriptor distance ratio test, which looks at the ratio of best vs. second-best match to decide whether to keep an associated pair of keypoints.

(matching2D_Student.cpp lines 37-50.) The threshold ratio was used as a parameter to the KNNMatch function call.

* MP.7 Performance Evaluation 1: Count the number of keypoints on the preceding vehicle for all 10 images and take note of the distribution of their neighborhood size. Do this for all the detectors you have implemented.

Below are the number of keypoints found for all the 10 images for each of the detectors.

Harris: 262, 263, 274, 279, 279, 271, 269, 270, 280, 276
ShiTomasi: 715, 680, 716, 705, 699, 672, 690, 711, 722, 698
FAST: 559, 532, 533, 546, 523, 520, 512, 503, 530, 530, 517
BRISK: 1472, 1494, 1480, 1467, 1487, 1450, 1463, 1412, 1417, 1429
ORB : 280, 277, 280, 282, 284, 288, 288, 289, 287, 292
AKAZE: 727, 710, 705, 724, 733, 724, 736, 720, 736, 725
SIFT: 771, 731, 734, 714, 702, 740, 749, 745, 790, 757

* MP.8 Performance Evaluation 2 : Count the number of matched keypoints for all 10 images using all possible combinations of detectors and descriptors. In the matching step, the BF approach is used with the descriptor distance ratio set to 0.8.

Below is the data of number of matches for all possible combinations of the detectors and descriptors for all 10 images.
 
SHITOMASI - BRISK : 268 239 252 242 248 242 256 256 252
SHITOMASI - BRIEF: 348 357 369 336 364 356 364 376 364
SHITOMASI - ORB: 307 308 342 309 323 298 317 333 321
SHITOMASI - FREAK: 257 256 272 239 249 246 246 256 248
HARRIS - BRISK: 107 106 119 139 118 115 119 131 126
HARRIS - BRIEF: 151 149 167 164 167 162 156 160 169
HARRIS - ORB: 143 124 145 158 148 146 145 155 153
HARRIS - FREAK: 113 111 107 131 118 127 122 122 120
FAST - BRISK: 232 210 229 218 208 201 192 215 231
FAST - BRIEF: 339 323 321 324 307 302 299 298 329
FAST - ORB: 296 275 290 288 288 271 257 281 308
FAST - FREAK: 223 204 199 212 216 195 180 208 222
BRISK - BRISK: 589 579 554 567 569 557 563 540 556
BRISK - BRIEF: 845 865 815 823 803 853 829 805 837
BRISK - ORB: 567 575 553 574 578 574 563 530 554
BRISK - FREAK: 569 558 536 573 560 573 557 536 567
ORB - BRISK: 146 156 154 145 152 155 148 162 160
ORB - BRIEF: 159 153 137 148 146 172 156 177 155
ORB - ORB: 156 147 159 154 158 173 168 173 164
ORB - FREAK: 69 58 66 76 65 72 75 67 82
AKAZE - BRISK: 379 359 353 331 349 367 362 369 370
AKAZE - BRIEF: 449 446 444 437 455 456 453 432 469
AKAZE - ORB: 385 367 364 335 364 383 376 365 397
AKAZE - FREAK: 351 341 351 328 348 363 373 404 364
SIFT - BRISK: 279 287 257 264 269 271 270 267 290
SIFT - BRIEF: 362 364 344 347 371 354 344 352 386
SIFT - FREAK: 290 287 268 265 267 256 266 263 285
AKAZE - AKAZE 390 380 384 378 380 407 378 391 375

* MP.9 Performance Evaluation 3: Log the time it takes for keypoint detection and descriptor extraction. The results must be entered into a spreadsheet and based on this data, the TOP3 detector / descriptor combinations must be recommended as the best choice for our purpose of detecting keypoints on vehicles.

SIFT - FREAK : 410ms
SIFT - BRIEF : 340ms
SIFT - BRISK : 820ms
AKAZE - ORB : 240ms
AKAZE - FREAK : 260ms
AKAZE - BRIEF : 190ms
AKAZE - BRISK : 980ms
ORB - ORB : 40ms
ORB - FREAK : 80ms
ORB - BRIEF : 10ms
ORB - BRISK : 590ms
BRISK - ORB : 750ms
BRISK - FREAK : 850ms
BRISK - BRIEF : 750ms
BRISK - BRISK : 129 ms
FAST - ORB : 10 ms
FAST - FREAK : 080 ms
FAST - BRIEF : 10 ms
FAST - BRISK : 580 ms
HARRIS - ORB : 20 ms
HARRIS - FREAK : 120 ms
HARRIS - BRIEF : 20 ms
HARRIS - BRISK : 600 ms
SHITOMASI - ORB : 40 ms
SHITOMASI - FREAK : 130 ms
SHITOMASI - BRIEF : 60 ms
SHITOMASI - BRISK : 650 ms
AKAZE - AKAZE : 480 ms

Based on the data above: The top 3 combinations would be FAST-ORB, FAST-BRIEF, ORB-BRIEF.