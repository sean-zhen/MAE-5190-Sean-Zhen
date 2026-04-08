---
layout: project
title: Lab 8
description: Lab 8
image: /assets/images/cat.jpg
---

<h3>Stunts!</h3>

---------------------------------------

fast stunts. Your grade will be based partially on your hardware/software design and partially on how fast your robot manages to complete the stunt (relative to everyone else in class). 

Controlled Stunts

Pick one of the following tasks. 

We will need video evidence that your stunt works at least three times, and graphs showing the sensor data, KF output (if applicable), and motor values with time stamps.

Task B: Drift

Your robot must start at the designated line (<4m from the wall), drive fast forward, and when the robot is within 3ft (914mm = 3 floor tiles in the lab) from the wall, initiate a 180 degree turn.

    The TOF sensors are not all calibrated equally well; manually tuning the distance to the wall for which your robot initiates the turn is fine.
    If you are using the Kalman Filter to estimate the distance to the wall quickly, do the following. When you have new sensor readings, run both the prediction and the update step. When you don’t have new sensor readings, run only the prediction step (i.e. compute your estimated distance based only on the motor inputs and your dynamics model). Remember to adjust your discrete A and B matrices to the new sample rate and your input to the new PWM values.


Extra credit for coolest stunt and best bloopers!

Everyone, including your teaching team, will be given the chance to vote for the coolest stunt video and the best blooper. We will send you a link to vote after Spring break. The highest scores will be given up to 2 extra credit points!

To demonstrate that you’ve successfully completed the lab, please upload a brief lab report (<800 words), with code snippets (not included in the word count), photos, graphs, and/or videos documenting that everything worked and what you did to make it happen.