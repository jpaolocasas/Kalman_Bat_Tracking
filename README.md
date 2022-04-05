# Kalman_Bat_Tracking
Code used for an assignment in CS585 (Image and Video Computing) at Boston University with instructor Margrit Betke. Given a video of bats flying, used the Kalman Filter to track their trajectories.

## Problem Definition
For this assignment we were tasked with tracking multiple objects on a greyscale video of bats flying. No specific methods were required but we were encouraged to experiment with both basic and more advanced methods to get experience with creating a multi-object tracker.

## Method and Implementation
To tackle the bat data set, the main components of the algorithm consist of a Kalman filter and the Hungarian algorithm. The Kalman filter is a widley used algorithm for object tracking that factors in measurement inaccuraccies and model uncertainty to predict trajectories. The Hungarian algorithm is a combinatorial optimization algorithm that takles assignment matching problems. An example of an assignment matching problem is how do you assign n employees to n tasks while minimizing the cost of paying the employees. For this particular scenario, the Hungarian algorithm is used to match n tracks to n predictions from the Kalman filter using the pixel-wise distance between the two as the cost metric. The general workflow is as follows:

- Read frame
- Blur and threshold frame with adaptive thresholding
- Calculate centroids of contours
- If it is the first frame, initialize a Kalman filter class for each centroid detected and call predict()
- For each Kalman filter assigned to an active object, call predict()
- Compute a cost matrix via euclidean distance between the centroids measured in the current frame and the predictions
- Feed the cost matrix to the Hungarian algorithm, munkres(), to match the centroids to existing tracks
- If the distance between the new measured centroid and the last recorded centroid is greater than a threshold, delete the existing track and start a new track based on    the new centroid
- If the index of the measured centroid that is returned from the Hungarian algorithm is greater than the current number of tracks, assume this is a new object entering    the frame
- Plot the predicted paths if the object has a flight history greater than n frames

## Results
