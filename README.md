# Path Planning
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

---

Udacity Self-Driving Car Engineer Nanodegree

Project 7

---

## Introduction

![Result](https://github.com/Ansheel9/Path-Planning/blob/master/extra/higway_2.gif) | ![Result](https://github.com/Ansheel9/Path-Planning/blob/master/extra/higway_3.gif) | ![Result](https://github.com/Ansheel9/Path-Planning/blob/master/extra/higway_6.gif)

In this project, the goal is to design a path planner that is able to create smooth, safe paths for the car to follow along a 3 lane highway with traffic. The path planner is able to keep inside its lane, avoid hitting other cars, and pass slower moving traffic all by using localization, sensor fusion, and map data. The car transmits its location, along with its sensor fusion data, which estimates the location of all the vehicles on the same side of the road.

## Dependencies

* cmake >= 3.5
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `install-mac.sh` or `install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./path_planning`.

## Model Description 

![Result](https://github.com/Ansheel9/Path-Planning/blob/master/extra/HighwayPathPlanner.png)

#### States Description

-  Keep Lane

This is the default state of the system, this state represents, that the car need to be in the same lane, and not need to change the lane, when possible, try to accelerate and reach the max allowed speed limit of 50mph.

- Prepare Lane Change

This state represents the status when car need to prepare for lane change since the other car in front is driving slower than the speed limit. In this state, car also tries to find out the best lane to change, and best time to change lane safely and efficiently.

- Change Lane : Left

During this state, car changes lane, and then speeds up to reach max allowed speed limit. In this state car also takes the stablization period of 10 simulation cycle (~1sec), before making next lane change, this allows car to be stable.

- Change Lane : Right

During this state, car changes lane, and then speeds up to reach max allowed speed limit. In this state car also takes the stablization period of 10 simulation cycle (~1sec), before making next lane change, this allows car to be stable.


This state is different than left lane change, since it gets priority when cost of changing lane becomes same for left and right. The logic is to avoid reaching to left lane which is also considered as the fast lane for highways.

#### Information In-outs

We are provided with the different way points around the track to follow. 
- Simulator expects:
    - The sequence of x,y coordinates to follow. The simulator covers each point in 0.02 sec, so the spacing between points, decides the speed of the car.
- Simulator provides following information:
    - Car's position in global x,y coordinates
    - Car's position in Frenet coordinate system
    - Car's velocity and yaw 
    - It also provides the list of remaining points, which are not yet visited by simulator and provided as last path points.

#### Path generation steps

- Our main goal is to create a path which is free from jerks and acceleration limits. For that the most important point to remember is, the path should be continuous, means we should try to utilize the previous path points which were provided to simulator.
- To ease the calculations we convert the global coordinates to car coordinates, and at the end revert from car to global coordinates.
- We chose the different way points, which are atleast 30m apart and then pass those points to spline to fit into a smooth curve.
- By using the spline we try to fit approx 50 path points for simulator to follow by providing the spaced points x points and getting corresponding y points.
- Once we have this vector of points available then we convert them to global coordinates and pass to simulator.

## Result

![training_img](https://github.com/Ansheel9/Path-Planning/blob/master/extra/result.PNG)

The car moves without colliding with other objects or surrounding.
