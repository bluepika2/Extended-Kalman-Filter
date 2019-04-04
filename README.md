# Extended Kalman Filter Project 

In this project I am going to utilize a extended Kalman filter in C++. Provided simulated lidar and rader measurements detecting bicycle that travels arouond the vehicle, the Kalman filter tracks the bicycle's position and veloicty from estimating the state of a moving object of interest with noisy lidar and radar measurements. 

[//]: # (Image References)

[image1]: ./output_images/KalmanFilter_data.png "data"
[image2]: ./output_images/ekf_flow.jpg "flow"
[image3]: ./output_images/ekf_vs_kf.jpg "vs"

## Explanation of data file

Here is a screenshot of data file:

![alt text][image1]

Each row represents a sensor measurement where the first column tells if the measurement come from either radar or lidar.
For a row containing radar data, the columns are: sensor_type, rho_measured, phi_measured, rhodot_measured, timestamp, x_groundtruth, y_groundtruth, vx_groundtruth, vy_groundtruth, yaw_groundtruth, yawrate_groundtruth.
For a row containing lidar data, the columns are: sensor_type, x_measured, y_measured, timestamp, x_groundtruth, y_groundtruth, vx_groundtruth, vy_groundtruth, yaw_groundtruth, yawrate_groundtruth.
Whereas radar has three measurements (rho, phi, rhodot), lidar has two measurements (x, y).
I will use the measurement values and timestamp in your Kalman filter algorithm. Groundtruth, which represents the actual path the bicycle took, is for calculating root mean squared error.

## Overview of a Kalman Filter: Initialize, Predict, Update
Let's discuss the three main steps for programming a Kalman filter:

1. initializing Kalman filter variables
2. predicting where our object is going to be after a time step Δt
3. updating where our object is based on sensor measurements
4. the prediction and update steps repeat themselves in a loop.

To measure how well our Kalman filter performs, we will then calculate root mean squared error comparing the Kalman filter results with the provided ground truth.
Here is flow of how the extended Kalman filter works:

![alt text][image2]

As shown above, extended Kalman filter will be utilized only for radar measurement. Other than that, normal Kalman filter will be leveraged.

Extended Kalman filter vs Kalman filter:

![alt text][image3]

## Files in `src` folder

`main.cpp` - communicates with the Term 2 Simulator receiving data measurements, calls a function to run the Kalman filter, calls a function to calculate RMSE
`FusionEKF.cpp` - initializes the filter, calls the predict function, calls the update function
`kalman_filter.cpp`- defines the predict function, the update function for lidar, and the update function for radar
`tools.cpp`- function to calculate RMSE and the Jacobian matrix

## How the Files Relate to Each Other
Here is a brief overview of running the code files:

`Main.cpp` reads in the data and sends a sensor measurement to `FusionEKF.cpp`
`FusionEKF.cpp` takes the sensor data and initializes variables and updates variables. The Kalman filter equations are not in this file. `FusionEKF.cpp` has a variable called `ekf_`, which is an instance of a `KalmanFilter` class. The `ekf_` will hold the matrix and vector values. I will also use the `ekf_` instance to call the predict and update equations.
The `KalmanFilter` class is defined in `kalman_filter.cpp` and `kalman_filter.h`. 

These three steps (initialize, predict, update) plus calculating RMSE encapsulate the entire extended Kalman filter project.
The main program can be built and run by doing the following from the project top directory.

1. mkdir build
2. cd build
3. cmake ..
4. make
5. ./ExtendedKF

INPUT: values provided by the simulator to the c++ program

["sensor_measurement"] => the measurement that the simulator observed (either lidar or radar)

OUTPUT: values provided by the c++ program to the simulator

["estimate_x"] <= kalman filter estimated position x
["estimate_y"] <= kalman filter estimated position y
["rmse_x"]
["rmse_y"]
["rmse_vx"]
["rmse_vy"]

## Results

In two different simulated runs, my Extended Kalman Filter produces the below results. The x-position is shown as 'px', y-position as 'py', velocity in the x-direction is 'vx', while velocity in the y-direction is 'vy'. Residual error is calculated by mean squared error (MSE).

|Input |MSE   |
|---   |---   |
|px    |0.0974|
|py    |0.0855|
|vx    |0.4517|
|vy    |0.4404|

## Other Important Dependencies

* cmake >= 3.5
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1 (Linux, Mac), 3.81 (Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)






