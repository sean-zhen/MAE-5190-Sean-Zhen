---
layout: project
title: Lab 7
description: Lab 7
image: /assets/images/Lab7_KalmanFilter.jpg
---

<h3>Kalman Filter</h3>

<div style="margin-top: 20px;"></div>
<h4>Task 1: Estimate drag and momentum</h4>

I set the distance sensor to long mode so I can place the robot as far as possible from the wall and receive reliable distance readings from the TOF sensor. My step response value is 125, which is similar to the PWM values used in Lab 5. I placed my robot 3.3 meters from the wall and stopped it when it was 150 mm from the wall. Because my robot wasn't able to achieve a steady state velocity with the space given, I used an exponential fit to extrapolate the velocity.  

<figure>
    <img src="{{ '/assets/images/Lab7_TOF.jpg' | relative_url }}"
        width="480">
</figure>
<figure>
    <img src="{{ '/assets/images/Lab7_Motor.jpg' | relative_url }}"
        width="480">
</figure>
<figure>
    <img src="{{ '/assets/images/Lab7_Speed.jpg' | relative_url }}"
        width="480">
</figure>

The steady state speed is <b>2.004 m/s</b>, the 90% rise time is <b>7.097 seconds</b>, and the speed at 90% rise time is <b>1.803 m/s</b>.

After sending the data back to my laptop, I saved the data in a CSV file. 

<figure>
    <img src="{{ '/assets/images/Lab7_CSV.jpg' | relative_url }}"
        width="480">
</figure>

<div style="margin-top: 20px;"></div>
<h4>Task 2: Initialize KF (Python)</h4>

The state space equation for the Kalman filter is:
<figure>
    <img src="{{ '/assets/images/Lab7_KalmanEq.jpg' | relative_url }}"
        width="480">
</figure>

To calculate d, I set `u = 1` and `x_dot` as the steady state speed.
<figure>
    <img src="{{ '/assets/images/Lab7_d.jpg' | relative_url }}"
        width="480">
</figure>

To calculate m, I used the following equation:
<figure>
    <img src="{{ '/assets/images/Lab7_m.jpg' | relative_url }}"
        width="480">
</figure>

So,
<figure>
    <img src="{{ '/assets/images/Lab7_AB.jpg' | relative_url }}"
        width="480">
</figure>

There were 101 datapoints in 10 seconds, so the sampling time is 0.09901 seconds (99.01 ms). 

Discretize matrices A and B:
<script src="https://gist.github.com/sean-zhen/8a6d98c148a4407371d6a56902b2b02c.js"></script>

<figure>
    <img src="{{ '/assets/images/Lab7_C.jpg' | relative_url }}"
        width="120">
</figure>
I am measuring the distance from the wall as positive. x is the state vector. `distance[0]` should be positive because I am treating all distances from the wall as positive.

We need to specify the process noise and sensor noise covariance matrices for the Kalman filter to work well. 
To calculate `sigma_1` and `sigma_2`, we use the sampling time in seconds. 
<figure>
    <img src="{{ '/assets/images/Lab7_sigma.jpg' | relative_url }}"
        width="480">
</figure>

`sigma_3` represents the measurement noise. Since we are using long range mode, the measurement noise according to the TOF sensor data sheet is 25 mm. 

<script src="https://gist.github.com/sean-zhen/c38542075c621357b90d99a74858c83d.js"></script>

<div style="margin-top: 20px;"></div>
<h4>Task 3: Implement and test your Kalman Filter in Jupyter (Python)</h4>

Using the function provided, I applied the Kalman filter to the data collected at the beginning of this lab. 

<figure>
    <img src="{{ '/assets/images/Lab7_InitialKF.jpg' | relative_url }}"
        width="480">
</figure>

This initial Kalman filter lags behind the actual signal, which means the filter is trusting the model too much. I can tune the filter by increasing the process noise. I increased `sigma_1` and `sigma_2` to 200. 

<figure>
    <img src="{{ '/assets/images/Lab7_FinalKF.jpg' | relative_url }}"
        width="480">
</figure>

Another parameter that affects the performance of the Kalman filter is the value of the measurement noise. If the value is too large, the filter will think the measurements are noisy and unreliable and will rely more on the model. 

<div style="margin-top: 20px;"></div>
<h4>Task 4: Implement the Kalman Filter on the Robot</h4>
I integrated the Kalman filter into my Lab 5 PID implementation. I placed the robot 3 feet away from the wall and had it stop 1 foot from the wall. The filter smooths out the noisy distance readings while matching the shape of the raw data. The robot was quick to start and stop at an accurate distance from the wall. This Kalman filter implementation makes the robot much quicker compared to just only using PID in Lab 5. The final values I used for this run is `Kp = 0.06`, `Ki = 0.25`, `Kd = 0.075`, and `sigma_1 = sigma_2 = 20`. I reduced the process noise so the Kalman filter predicts the distances rather than matching exactly to the raw distance data.

<figure>
    <img src="{{ '/assets/images/Lab7_RawKF.jpg' | relative_url }}"
        width="480">
</figure>

<iframe width="480" height="270" src="https://youtube.com/embed/rDKixASjbow" allowfullscreen> </iframe>

<div style="margin-top: 20px;"></div>
<h3>Collaborators</h3>
I collaborated with Sam Zhen while working on this lab.