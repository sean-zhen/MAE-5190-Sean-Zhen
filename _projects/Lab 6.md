---
layout: project
title: Lab 6
description: Lab 6
image: /assets/images/Lab6_Gyro.jpg
---

<h3>Orientation Control</h3>

<div style="margin-top: 20px;"></div>
<h4>Prelab</h4>
Similarly to Lab 5, when I run my command, I will send values of Kp, Ki, and Kd to the Artemis over Bluetooth by writing these values in my command in my Jupyter notebook. An example of the command I would send is: `ble.send_command(CMD.COMMAND_NAME, "1|2|3")`. This allows me to fine tune my closed loop controller without having to upload new code whenever I make any changes. 

In my Arduino code, I need to add this to the beginning of the case to receive the inputs:
<script src="https://gist.github.com/sean-zhen/cebc8ec9c079fcd92edcb47bd4d65f3f.js"></script>

I will implement a time out in my code so the robot automatically stops after a defined amount of time. I will send this time out value over Bluetooth as well. This time out will always run regardless of Bluetooth connection. 

I will initialize arrays when I run the controller and send these arrays to my computer over Bluetooth when the program is finished. The data that I'm collecting in these arrays are timestamps, gyroscope readings, and outputs from the branches of the PID controller for debugging. 
<script src="https://gist.github.com/sean-zhen/a289da7674d34f39209ee9898c84bdf2.js"></script>

<h3>Lab Tasks</h3>

Static friction on-axis turn re-calibration

Gyroscope calculation

---------------------------------------------------

Lab Tasks
    P/I/D discussion (Kp/Ki/Kd values chosen, why you chose a combination of controllers, etc.)
    Range/Sampling time discussion
    Graphs, code, videos, images, discussion of reaching task goal
    Graph data should at least include theta vs time (you can also consider angular velocity, motor input, etc)
    (5000) Wind-up implementation and discussion


5000-level students can choose between PI and PID controllers.

For this lab, we will work on implementing stationary (in place) orientation control. The control signal (output from the PID controller) will be a differential drive value. The wheel should be driven at the same speed in opposite directions in order to control the orientation of the robot


PID Input Signal

    You should integrate your gyroscope to get an estimate for the orientation of the robot.
    Are there any problems that digital integration might lead to over time? Are there ways to minimize these problems?
    Does your sensor have any bias, and are there ways to fix this? How fast does your error grow as a result of this bias? Consider using the onboard digital motion processor (DMP) built into your IMU to minimize yaw drift.
    Are there limitations on the sensor itself to be aware of? What is the maximum rotational velocity that the gyroscope can read (look at spec sheets and code documentation on github). Is this sufficient for our applications, and is there a way to configure this parameter?

Derivative Term

    Does it make sense to take the derivative of a signal that is the integral of another signal.
    Think about derivative kick. Does changing your setpoint while the robot is running cause problems with your implementation of the PID controller?
    Is a low pass filter needed before your derivative term?

Programming Implementation

    Have you implemented your code in such a way that you can continue sending an processing Bluetooth commands while your controller is running?
    This is essential for being able to tune the PID gains quickly.
    This is also essential for being able to change the setpoint while the robot is running.
    Think about future applications of your PID controller with regards to navigation or stunts. Will you need to be able to update the setpoint in real time?
    Can you control the orientation while the robot is driving forward or backward? This is not required for this lab, but consider how this might be implemented in the future and what steps you can take now to make adding this functionality simple.

Include graphs of all appropriate measurements needed to debug your PID controller. Below is an example the set point, angle and motor offset plotted as a function of time. Observe the overshoot and settling time of the angle and the response of the motor values.

Tasks for 5000-level students

Implement wind-up protection for your integrator. Argue for why this is necessary (you may for example demonstrate how your controller works reasonably independent of floor surface).

Word Limit: < 800 words