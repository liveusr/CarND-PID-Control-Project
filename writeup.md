# **PID Controller**

The goal of this project is to implement a PID controller in C++ and tune the PID hyperparameters to maneuver the vehicle around the track in simulation. The simulator provides CTE (cross track error) and the velocity (mph) in order to compute the appropriate steering angle and throttle.

## PID Control

PID Control is a control loop feedback mechanism in control systems. Simulator provides difference between reference trajectory and actual car position i.e. CTE. PID controller continuously takes this error value - CTE as an input and applies a correction based on proportional, integral and derivative terms. Following section describes effect of each component in PID controller.

### P - Proportional

Proportional component produces the correction value directly proportional to CTE. This proportion is decided by constant `Kp`. Increasing value of `Kp` increases responsiveness of controller. This helps to take sharp turns. However, if `Kp` is very large, car oscillates around the reference trajectory. Following images show effect of different values of `Kp`: 

| Kp = 0                                        | Kp = 1                                        | Kp = 0.1                                          |
|:---------------------------------------------:|:---------------------------------------------:|:-------------------------------------------------:|
| ![alt text](./writeup_data/Kp_0.gif "Kp = 0") | ![alt text](./writeup_data/Kp_1.gif "Kp = 1") | ![alt text](./writeup_data/Kp_0.1.gif "Kp = 0.1") |

### I - Integral

Integral component integrates past values of CTE over time and produces correction proportional to this residual error. This proportion is decided by constant `Ki`. This helps to quickly fix residual error after application of P component. Thus, helps to fix systematic bias like steering drift, or large CTE on sharp curves due to high speed. Increasing value of `Ki` causes car to overshoot. Following images show effect of different values of `Ki`: 

| Ki = 0                                        | Ki = 0.008                                            | Ki = 0.002                                            |
|:---------------------------------------------:|:-----------------------------------------------------:|:-----------------------------------------------------:|
| ![alt text](./writeup_data/Ki_0.gif "Ki = 0") | ![alt text](./writeup_data/Ki_0.008.gif "Ki = 0.008") | ![alt text](./writeup_data/Ki_0.002.gif "Ki = 0.002") |

### D - Derivative

Derivative component produces the correction value proportional to rate of change of CTE. This proportion is decided by constant `Kd`. This helps to minimize the overshooting, thus keeps vehicle relatively stable. Increasing value of `Kd` helps take turns faster, but if value is very large, it increases sensitivity. Following images show effect of different values of `Kd`:

| Kd = 0                                        | Kd = 5                                        | Kd = 1                                        |
|:---------------------------------------------:|:---------------------------------------------:|:---------------------------------------------:|
| ![alt text](./writeup_data/Kd_0.gif "Kd = 0") | ![alt text](./writeup_data/Kd_5.gif "Kd = 5") | ![alt text](./writeup_data/Kd_1.gif "Kd = 1") |

## Choosing Hyperparameters (P, I, D coefficients)

All the hyperparameters were tuned manually. This helped to understand effect of each parameter. Only one parameter was tuned at a time. To start with, `Kp` was set to 1 with other parameters set to 0. It caused lot of overshoot. After few trials, value 0.1 was chosen for `Kp`. After that `Kd` was set to 1. This minimized oscillations to a large extent. Higher values of `Kd` led to higher oscillations after taking a turn. After few trials, value 1 was chosen for `Kd`. Next, `Ki` was set to 1. This drove car off the track. Since Integral component combines past error values, it was important to set `Ki` to relatively lower value. After few trials, value of 0.0001 was chosen for `Ki`. Final values for `Kp, Ki, Kd` are `0.1, 0.002, 1` respectively.

PID controller for throttle is also implemented. Higher steering value indicates that the car is either taking a sharp turn or car is drifted from its reference trajectory with large magnitude and hence needs quick correction with higher steering value. So, steering value derived by above steering PID controller was fed to throttle PID controller. With this implementation, car could reduce speed on turns and applied brakes if required. All the parameters for throttle PID controller were also tuned manually in similar way. Final values for throttle `Kp, Ki, Kd` are `0.8, 0.0001, 0.2` respectively. As you can notice value of Kp is greater than value of Kd for throttle PID controller. This is because throttle PID controller takes steering value as input, which is already error corrected.

## Results
    
Following GIF shows snippet from actual test run in the simulator:

![alt text](writeup_data/output_video.gif "output_video")

Complete output video can be found here: [output_video.mp4](output_video.mp4)

