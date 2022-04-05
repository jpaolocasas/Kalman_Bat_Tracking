# Kalman_Bat_Tracking
Code used for an assignment in CS585 (Image and Video Computing) at Boston University with instructor Margrit Betke. Given a video of bats flying, used the Kalman Filter to track their trajectories.

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

https://user-images.githubusercontent.com/50607673/161849929-cffe7d4f-8582-402d-bf12-f0ce59779400.mp4

## Discussion

Based on the video, a challenging situation when the tracker succeeds is when a new object enters the frame, although there is a slight lag sometimes. My implementation handles this case by checking to see if any of the ideal indices returned by the Hungarian algorithm are outside of the existing number of tracks. If it is outside of the range, then I make the assumption that it is a new object and initalize a new Kalman filter to track it. A challenging situation where my tracker fails are when the objects touch or if there are spurious detections. The spurious detections case isn't too bad as the predicted measurements are fairly accurate but you can sometimes see little zig-zags when spurious measurements are present. However, it is very common to see the trajectories switch between objects when the objects become too close to one another. Since I make the assumption that a prediction-centroid pairing greater than a distance n is incorrect, it is common to see a new track start where an old track ends for the same object. Another shortcoming of my implementation is that the play-back speed gets pretty slow. This is likely due to the fact that the Hungarian Algorithm has a O(n^4) run time at worst. Thus, as the more birds fill the screen and more "centroids" are added due to the birds wings extending, the processing can take awhile.
