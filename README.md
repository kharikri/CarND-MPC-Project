# MPC Controller

## Introduction

This is the fifth project in the second term of the Self Driving Car Nanodegree course offered by Udacity. In this project, I implemented an MPC controller to maneuver a vehicle around a simulated track. [Model predictive controllers](https://en.wikipedia.org/wiki/Model_predictive_control) (MPC) rely on dynamic models of the process. The main advantage of MPC is the fact that it allows the current timeslot to be optimized, while keeping future timeslots in account. MPC has the ability to anticipate future events and can take control actions accordingly.

## The Model

I use the kinematic model covered in the course. The current state has x and y coordinates, velocity, and heading direction (x, y, v, ψ) of the vehicle. It also has actuator values, acceleration (a) and change in heading (δ). The state is updated using standard motion equations shown below:

![alt text](https://github.com/kharikri/CarND-MPC-Project/blob/master/equations/StateEq1.png)

Lf is a constant based on the turning radius of the car and dt is the time difference between the state at t+1 and at t. 

There are also two error components, eψ and cte. The eψ is the difference between the calculated angle of the road and the heading. The cte, is the cross-track-error, or the distance from the center of the car to the center of the road. Both of these are calculated by comparing the car's actual location and heading with that predicted by the fitted polynomial of the road waypoints, in the following way:

![alt text](https://github.com/kharikri/CarND-MPC-Project/blob/master/equations/StateEq2.png)

## Timestep Length and Elapsed Duration (N & dt)

The timestep length N is how many states we *lookahead* in the future and the time step frequency dt is how much time elapses between actuations. The values chosen for N and dt are 10 and 0.1 respectively. These values were recommended in the tutorial. A smaller N does not take advantage of the look-ahead aspect of the algorithm and the ride is erractic. A large N was computationally expensive. N equal to 10 and dt eqial to 0.1 was the best option to keep the vehicle on the track.

## Polynomial Fitting and MPC Preprocessing

The waypoints are transformed from map to the vehicle's space. After doing this, the waypoints are in reference to the vehicle and its  x and y coordinates are at the origin (0, 0) and the orientation angle is also zero.

I then fit these transformed waypoints with a third degree polynomial.

## Model Predictive Control with Latency

The original kinematic equations as depicted above depends on the actuations from the previous timestep, but with a delay of 100ms the actuations are further pushed out.  Hence to account for this latency I use previous actuations. 

---

## Dependencies for Windows 10

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run ./install-ubuntu.sh. Check [here](https://github.com/kharikri/CarND-Kidnapped-Vehicle-Project/blob/master/WebSocketInstallation.md) if you run into issues with websocket installation for windows 10
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

## Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./mpc`

