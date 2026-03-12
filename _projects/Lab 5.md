---
layout: project
title: Lab 5
description: Lab 5
image: /assets/images/cat.jpg
---

<h3>Linear PID Control and Linear Interpolation</h3>

<div style="margin-top: 20px;"></div>
<h4>Prelab</h4>
When I start my robot, I will send values to the Artemis, such as Kp and Ki, over Bluetooth by writing these values in the command on my Jupyter notebook. An example of the command I would send is: `ble.send_command(CMD.COMMAND_NAME, "1|2|3")`. This allows me to fine tune my closed loop controller without having to upload new code whenever I make any changes. 

In my Arduino code, I need to add this to the beginning of the case to receive the inputs:
<script src="https://gist.github.com/sean-zhen/cebc8ec9c079fcd92edcb47bd4d65f3f.js"></script>

I will implement a time out in my code so the robot automatically stops after a defined amount of time. I will send this time out value over Bluetooth as well. This time out will always run regardless of Bluetooth connection. 

I will initialize arrays when I run the controller and send these arrays to my computer over Bluetooth when the program is finished. The data that I'm collecting in these arrays are timestamps, sensor distance data, and outputs from the branches of the PID controller. The setup values will also be stored in variables and will be outputted after completion. 

<script src="https://gist.github.com/sean-zhen/a289da7674d34f39209ee9898c84bdf2.js"></script>

<div style="margin-top: 20px;"></div>
<h4>Task 1: Position Control</h4>

Since I am operating the ToF sensors in short range mode, the maximum range I can get is 1.3 meters (4.27 ft). If I start my robot 1.3 meters away from the wall and want to have the robot stop when it is 0.304 meters away, the maximum error I can expect is 0.996 meters. The values from the sensors is outputted in mm. Since the maximum PWM value I can apply to the motors is 255 for 100% speed, the maximum Kp value for this setup will be `255 / 996 mm = 0.256`. 

The final values I chosen for my PID controller are: `Kp = 0.05`, `Ki = 0.11`, and `Kd = 0.0075`.

I initially started with PI, but because of my large deadband value I had to implement for my robot, I was not able to make the robot stop 1 foot from the wall within 10 seconds. If my Ki was too low, my robot would stop early and far from the wall. If my Ki was too high, the robot would overshoot and hit the wall. 

Because the error accumulates and continues to increase if the error stays positive, I had to implement a Kd term to counteract the integrator term. This allows the robot to slow down as it approaches the target distance, but also allows the integrator term to make final adjustments after the robot initially stops.

I implemented code to eliminate the initial derivative kick. When the derivative term compares the current error to the last error, the difference is very high for the first reading because the last error is initialized to 0. To solve this issue, I set the last error to be the same as the current error for the first calculation. The derivative term would be equal to 0 for the first calculation, and then calculates normally afterwards. 

I decided not to include a low pass filter for the derivative term because the data was not  noisy to be significant enough to affect the output for the motors. 

<div style="margin-top: 20px;"></div>
<h4>Task 2: Extrapolation</h4>

<div style="margin-top: 20px;"></div>
<h4>Tasks for 5000-level students</h4>
I implemented wind-up protection for the integrator. This is necessary because the total error accumulates and continues to increase when the error remains positive. This can cause the integrator term to be excessively large and can dominate over the Kp and Kd terms. To prevent the integrator term from over-saturating the controller, I clamped the integrator term if it exceeded the value of 255. This limits the integrator term to a maximum of 100% speed to the controller. 

<div style="margin-top: 20px;"></div>
<h3>Collaborators</h3>
I collaborated with Sam Zhen while working on this lab.

--------------------------
    Lab Tasks
        Range/Sampling time discussion
        Graphs, code, videos, images, discussion of reaching task goal
        Graph data should include Tof vs time and Motor input vs time (and whatever helps with debugging)
        (5000) Wind-up implementation and discussion

Your solution should be robust to changing conditions, such as the starting distance from the wall (2-4m). 

You must also demonstrate that your controller is robust to external perturbations. If you push the car further from the wall, and then towards the wall, it should return to the desired setpoint.

Tips and tricks:
    Start simple: E.g. with a proportional controller running at low speeds and a generous setpoint, then you can work your way up to faster speeds, more advanced control, and more difficult setpoints if you have time.
    
    Documentation: Please clearly document the maximum linear speed you are able to achieve (you can use your sensors to compute this). To demonstrate reliability, please upload videos of at least three repeated (and hopefully successful) experiments.
    
    Frequency: Fast loop times mean everything to a good controller. Be sure to include a discussion of sensor sampling rate and how this affects the timing of your control loop. Avoid using blocking statements when you can (e.g. delay() or while(sensor not ready) ){wait} ). 

    Wind up: If you include an integrator, consider whether you need to worry about integrator wind-up.
    
Extrapolation

A simple but less accurate alternative is a data extrapolator.

Write a function to extrapolate new TOF values based on recent sensor values, such that you can drive your robot quickly towards the wall with high accuracy. Be sure to demonstrate that your solution works by uploading videos and figures that plot corresponding raw and estimated data in the same graph.
Instructions:

    Determine the frequency at which the ToF sensor is returning new data.
        This is likely the rate at which your PID control loop is running as well. We want to decouple these two rates.
    Change your loop to calulate the PID control every loop, even if there is no new data from the ToF sensor.
        Check if new data from the ToF sensor is ready. If it is, update the variable that PID controller is using to estimate the motor speed.
        If a new datapoint isn’t ready, recalculate the PID control using using the last saved datapoint.
        The net effect this should have on your system should be the same. You PID control should now be running faster than your ToF sensor is generating new data.
    How fast is the PID control loop running? Compare this rate to ToF sensor rate.
    Rather than use an old datapoint to calulate the PID control, extrapolate an estimate for the car’s distance to the wall using the last two datareadings from the ToF sensor.
        Calcuate the slope from the last two datapoint, and extrapolate the current distance based on the ammount of time that has passed since the last reading and the slope.
        This is a simple linear extrapolation algorithm. Everytime you get a new ToF reading, use it along with the previous reading to estimate the current distance to the wall untill a new reading is recieved. If you have any questions about this, please ask one of the TAs for clarification.

Tasks for 5000-level students

(you may for example demonstrate how your controller works reasonably independent of floor surface). Demonstrate your system with and without wind-up protection.