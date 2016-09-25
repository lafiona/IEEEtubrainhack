# IEEEtubrainhack

First order of business: getting raw data from Thalmic Labs Myo

We took a MEX wrapper from a Mathworks file exchange, and ran through the example codes to figure out which data are available for use in MATLAB.

After looking through EVERY variable, we condensed the list down to: EMG data from 8 different channels and rotation+gyroscope data used to calculate orientation of armband in all 3 axes.

For EMG signal, we took a portion of the latest data points (last 100 points of 200Hz signal) and took the power to give a scalar value that we could plot. Note that we slide the window for every data point, so we are not missing any information. Then, we trained a set of 7 muscle movements from forearm EMG signals (resting, bend wrist left, bend wrist right, fist, hold up two fingers, spread fingers out, spiderman hand) using the averaged power data. The training only takes 10 for each movement.

Then, we read in test EMG values and saw if we could classify movement based on our training set. We used a least squared error classification method, and it turned out to be computationally feasible and pretty functional. Then, we read in the rotational and gyroscope data and use some calculations to convert it to orientation, we did absolute thresholding to separate forearm position into "down," "middle," "high." We then convert the movemenet classification plus orientation data into a binary number output for the bluetooth module into the arduino, which reads it in as serial data and decides various states (pre-defined in C, uploaded to the arduino).

The robot, as we said, was coded in C, and had controllable DC motors (both speed and direction) and 3 servos. We were originally planning on moving the servos in the rhythm that we provide a visually evoked potential, but we were unable to get meaningfully low-noise signal, so we opted to focusing on the EMG. The different states were stationary, moving forward, turning left while moving, turning right while moving, turning left in place, turning right in place, and going backwards. We also had commands on the Myo that could change the speed of the motor based on how many fingers we are holding up.
