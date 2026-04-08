---
layout: project
title: Lab 8
description: Lab 8
image: /assets/images/cat.jpg
---

<h3>Stunts!</h3>

My robot will be performing <b>Task B: Drift</b>. 

The robot will do the following to perform the drift:
1. Drive forward towards the wall with open-loop control until the target distance is reached.
2. Start an differential turn where one motor spins forwards while the other spins backwards. Keep turning until the yaw reading from the IMU data reads the target heading. 
3. Spin both motors forward to drive away from the wall for a few seconds. 

<u>Step 1: Drive forward</u>

<script src="https://gist.github.com/sean-zhen/8ff4cfd45f83c6c7d4c57b50f2eca46d.js"></script>

I removed the delay in the distance sensor reading so the loop time is quick and allows the gyroscope reading to update even when the distance sensor doesn't have new data. 

The `target_distance ` is 900 mm. The `left_pwm` value is 130 while the `right_pwm` value is 145.

The `step_one_flag` variable is first initialized as `True` to allow the if statement to run. Once the distance from the TOF sensor is less than the target distance, the flag is set to `0` or `False` so that if statement does not run again, even if the distance were to become greater than the target distance again. 

<u>Step 2: Turn</u>

<script src="https://gist.github.com/sean-zhen/dd3e57e498740597f893a4ebbddbe393.js"></script>

The `target_angle` is 75 degrees. The `left_turn` value is 250 and the `right_turn` value is 200. 

Similar to step 1, the `step_two_flag` is created so the if statement does not run again when the target angle is reached and the code moves on to Step 3. 

<u>Step 3: Drive away</u>

<script src="https://gist.github.com/sean-zhen/d7a15ce6fe3538bbbbd67a866ff6468d.js"></script>

Step 3 uses the same PWM values of 130 for `left_pwm` and 145 for `right_pwm` from Step 1 to drive away from the wall. The robot drives away until the program times out, which is set for 5 seconds. 

<u>Videos</u>

I taped folders to the floor to create a slick surface so the robot can drift easier. The laminate floor is too rough for the robot to drift. On the wood floor, the robot would look like it was doing an on-axis turn instead of drifting. 

In each video, the robot is starting around 3 meters from the wall. 

<iframe width="480" height="270" src="https://youtube.com/embed/cdEgCuIdHBk" allowfullscreen> </iframe>

<iframe width="480" height="270" src="https://youtube.com/embed/XUMJypNL5WQ" allowfullscreen> </iframe>

<iframe width="480" height="270" src="https://youtube.com/embed/sjTbGfgrUcQ" allowfullscreen> </iframe>

<u>Graphs</u>

Below are the graphs for one of the runs:
<figure>
    <img src="{{ '/assets/images/Lab8_Distance.jpg' | relative_url }}"
        width="480">
</figure>
The red vertical dashed line represents the time `T = 45046 ms` when the distance sensor first provided a distance less than 900 mm. 

<figure>
    <img src="{{ '/assets/images/Lab8_Yaw.jpg' | relative_url }}"
        width="480">
</figure>
The green vertical dashed line represents the time `T = 45387 ms` when the calculated yaw angle is 75 degrees. The graph shows that the robot continues to turn even after both motors are spinning forward in the same direction. The max yaw angle appears to be around 120 degrees, even though the robot made a roughly 180-degree turn. This is most likely caused by the robot turning too quickly, which exceeded the max angular velocity that the gyroscope can handle. 

<figure>
    <img src="{{ '/assets/images/Lab8_Motor.jpg' | relative_url }}"
        width="480">
</figure>

The left motor is reversed while the right motor continues to drive forward to make the robot turn and drift.

<h4>Discussion</h4>

Open-loop control allows for easy tuning of target distance and headings for the robot to successfully drift. The videos show that the robot does not hit the wall in each of the three runs and the robot was able to complete a roughly 180-degree turn. I decided that a Kalman filter was not necessary for the distance sensor data as the robot was able to react quickly and initiate the turn without hitting the wall. A PID loop for the heading control would allow for a more precise turn, but this would take extra time for the robot to stabilize to a setpoint. Instead, I used an open-loop control for the turn until a specific target heading was reached, which appears to be very quick compared to a PID controller. 

<div style="margin-top: 20px;"></div>
<h3>Collaborators</h3>
I collaborated with Sam Zhen while working on this lab.