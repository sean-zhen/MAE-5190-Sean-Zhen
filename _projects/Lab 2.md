---
layout: project
title: Lab 2
description: Lab 2
image: /assets/images/9DOF_IMU.jpg
---

<h4>Set up the IMU</h4>
After downloading the “SparkFun 9DOF IMU Breakout_ICM 20948_Arduino Library” from the Arduino Library Manager, I connected the IMU to the Artemis board using the QWIIC connectors. 
<figure>
    <img src="{{ '/assets/images/Lab2_IMU_Connections.jpg' | relative_url }}"
        width="480">
</figure>

I ran the `Example1_Basics` code, and the video shows the accelerometer and gyro values changing as I rotate the IMU in different directions. 

When the board is upright with Z up, the third accelerometer value is positive. When the board is Z down, this value becomes negative. I was also rotating the board about the X-axis, which is represented by the change in the first gyro value. Rolling the board such that X is up make the first accelerometer value positive, negative when X is down. Whether these values are positive or negative depends on the sign convention defined by the diagram on the IMU.
<iframe width="480" height="270" src="https://youtube.com/embed/o5yLr7anUFY" allowfullscreen> </iframe>

The AD0_VAL should be set to 1. The value becomes 0 when the ADR jumper on the IMU is closed. Since there is no jumper, the value should be 1.

I also added code in `void setup()` for the board to blink the LED three times when it powers on.
<iframe width="480" height="270" src="https://youtube.com/embed/CBKmRWoHwdA" allowfullscreen> </iframe>
<script src="https://gist.github.com/sean-zhen/0cb1b8fe99750c11b9aa348a1911360a.js"></script>

<div style="margin-top: 20px;"></div>
<h4>Accelerometer</h4>
The following code snippet shows the equations used to calculate pitch and roll:
<script src="https://gist.github.com/sean-zhen/157a71afe94b197e83b60f9962db9c7d.js"></script>

Pitch: 90 degrees
<figure>
    <img src="{{ '/assets/images/Lab2_Pitch90.jpg' | relative_url }}"
        width="480">
</figure>

Pitch: -90 degrees
<figure>
    <img src="{{ '/assets/images/Lab2_Pitch-90.jpg' | relative_url }}"
        width="480">
</figure>

Pitch: 0 degrees and Roll: 0 degrees
<figure>
    <img src="{{ '/assets/images/Lab2_Pitch0Roll0.jpg' | relative_url }}"
        width="480">
</figure>

Roll: 90 degrees
<figure>
    <img src="{{ '/assets/images/Lab2_Roll90.jpg' | relative_url }}"
        width="480">
</figure>

Roll: -90 degrees
<figure>
    <img src="{{ '/assets/images/Lab2_Roll-90.jpg' | relative_url }}"
        width="480">
</figure>

I wrote a function in Jupyter to plot the data and time on a graph.
<script src="https://gist.github.com/sean-zhen/57f5251b88c6a0effa0a244ca5bc9582.js"></script>
<figure>
    <img src="{{ '/assets/images/Lab2_PlotData.jpg' | relative_url }}"
        width="480">
</figure>

When the IMU is rolled and pitched to either 90 or -90 degrees, the output is off by one or two degrees. 

For pitch, the average output is 89.084 degrees at the expected value of 90 degrees, the average output is -88.404 degrees at the expected value of -90 degrees. The calibration formula should be `Corrected = (180 / 177.488) × Raw + (90 – (180 / 177.488) × 89.084)`

For roll, the average output is 88.652 degrees at the expected value of 90 degrees, the average output is -88.802 degrees at the expected value of -90 degrees. The calibration formula should be `Corrected = (180 / 177.454) × Raw + (90 – (180 / 177.454) × 88.652)`

The following code performs a Fourier Transfrom of the data.
<script src="https://gist.github.com/sean-zhen/5f7b0787a1b28128e7bc573e61d8aec3.js"></script>

<figure>
    <img src="{{ '/assets/images/Lab2_FFTRoll.jpg' | relative_url }}"
        width="480">
</figure>
<figure>
    <img src="{{ '/assets/images/Lab2_FFTPitch.jpg' | relative_url }}"
        width="480">
</figure>

A high cutoff frequency leaves in more noise in the signal/data, while a low cutoff noise can smooth the signal too much and distort the original signal. 

Let's set the cutoff frequency (f_c) to 5 Hz. With f_c = 1 / (2πRC), RC = 0.03183. From the graphs, T = 0.003893, so alpha = T / (T + RC) = 0.10897.

<script src="https://gist.github.com/sean-zhen/26302dc32b6af8c71d1afeb8f5c370aa.js"></script>

<figure>
    <img src="{{ '/assets/images/Lab2_LPFRoll.jpg' | relative_url }}"
        width="480">
</figure>
<figure>
    <img src="{{ '/assets/images/Lab2_LPFPitch.jpg' | relative_url }}"
        width="480">
</figure>

The low-pass filter smooths the signal and removes most of the high-frequency noise.

<div style="margin-top: 20px;"></div>
<h4>Gyroscope</h4>
The following code calculates the pitch, roll, and yaw of the IMU based on the gyroscope. dt is in milliseconds and is calculated as the time between the current and last loop time. The initial reading starts at 0, so the angles are relative, not absolute.
<script src="https://gist.github.com/sean-zhen/6b0f00482a883b99525c82b3ccdaf316.js"></script>
<figure>
    <img src="{{ '/assets/images/Lab2_GyroPitch.jpg' | relative_url }}"
        width="480">
</figure>
<figure>
    <img src="{{ '/assets/images/Lab2_GyroRoll.jpg' | relative_url }}"
        width="480">
</figure>
<figure>
    <img src="{{ '/assets/images/Lab2_GyroYaw.jpg' | relative_url }}"
        width="480">
</figure>
The gyro has significant drift. I was rotating and turning the IMU multiple times, which can explain the large error between the gyro and accelerometer readings. Also, the gyro reading starts at 0, which explains the vertical offset between the readings. 

For the complementary filter, I used an alpha of 0.75 to rely more on the filtered accelerometer values rather than the gyroscope because of the gyro is easily affected by drift. A lower alpha would rely more on the gyro.

<script src="https://gist.github.com/sean-zhen/ecf409311af0cc73a3c9445fc33cf583.js"></script>
<figure>
    <img src="{{ '/assets/images/Lab2_PitchComp.jpg' | relative_url }}"
        width="480">
</figure>
<figure>
    <img src="{{ '/assets/images/Lab2_RollComp.jpg' | relative_url }}"
        width="480">
</figure>

For pitch, the complementary filter values appear to be closer to the filtered accelerometer values than the gyro values. For roll, it appears that the complementary filter values are still affected by the gyro's drift but to a lesser extent than just gyro alone.

<div style="margin-top: 20px;"></div>
<h4>Sample Data</h4>
I did not use `if(myICM.dataReady())` in my code. Instead, I just used `myICM.getAGMT()`. There are no Serial.print statements in my code. The following code calculates and collects the pitch and roll values from the accelerometer and gyroscope along with the timestamps. I created 5 individual arrays to store these values. This makes it easier to access element i of each list using the same index number. This ensures all of the data corresponds to the same timestamp. Because there is math during the calculations of the values, it is best to set the data type of each array as a float. I set the array length to 10,000 to ensure that the array does not become full, and I collected the data for 5 seconds.
<script src="https://gist.github.com/sean-zhen/274103605bbf01210ada69de5645827c.js"></script>
Over 5 seconds, the Artemis sent 2023 messages in the format of: `Time [ms]: 263489.000 | Pitch (Filtered Accel): -11.792 | Roll (Filtered Accel): -0.099 | Pitch (Gyro): -4.568 | Roll (Gyro): 1.702`. This message is 131 bytes long, which means the board sent 53,002.6 bytes/second. It appears that each message has unique values, so the Artemis is not running faster than the IMU can produce new values. With a speed of roughly 53 kB/s and a RAM of 384 kB, the Artemis can store about 7.25 seconds of data. 

The first message is `Time [ms]: 258490.000 | Pitch (Filtered Accel): -1.954 | Roll (Filtered Accel): -0.109 | Pitch (Gyro): -0.000 | Roll (Gyro): -0.000`
The last message is `Time [ms]: 263489.000 | Pitch (Filtered Accel): -11.792 | Roll (Filtered Accel): -0.099 | Pitch (Gyro): -4.568 | Roll (Gyro): 1.702`

<div style="margin-top: 20px;"></div>
<h4>Record a Stunt</h4>
<iframe width="480" height="270" src="https://youtube.com/embed/ZKJ0wVvLMZo" allowfullscreen> </iframe>
The car accelerates and turns very quickly. A quick acceleration forward and then a quick acceleration in reverse would cause the car to flip. Holding down left and forward at the same time makes the car turn about one of the tires instead of the center of the car. Towards the end of the video, I lost control of the car.

<div style="margin-top: 20px;"></div>
<h3>Collaborators</h3>
I collaborated with Sam Zhen while working on this lab.