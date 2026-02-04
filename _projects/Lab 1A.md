---
layout: project
title: Lab 1
description: Lab 1
image: /assets/images/SparkFun_RedBoard_Artemis_Nano.jpg
---

### <u>Lab 1A</u>
<div style="margin-top: 20px;"></div>
<h4>Prelab</h4>

I installed the Arduino IDE on my laptop and added the SparkFun Apollo3 boards manager.  

<h4>Tasks</h4>
1. I connected the Artemis board to my laptop and selected Redboard Artemis Nano as the board. 
    <figure>
        <img src="{{ '/assets/images/Lab1A_Task1.JPG' | relative_url }}"
            width="480">
    </figure>

2. I ran the `Blink` example to test the LED. The blue LED on the board turns on for 1 second and turns off for another second repeatedly.  
    <iframe width="480" height="270" src="https://youtube.com/embed/OgtFOxUEX8U" allowfullscreen> </iframe>

3. I ran the `Example4_Serial` example to test the serial printing. I set the baud rate in the Serial Monitor to 115200 baud and made sure it is the same value in the sketch. I typed in "Test" as the message, and the serial monitor printed "Test".
    <figure> <img src="{{ '/assets/images/Lab1A_Task3.jpg' | relative_url }}" width="480">
    </figure>

4. I ran the `Example2_analogRead` example to retrieve the analog value of the chip's temperature. I placed my finger on the chip of the Artemis Nano board. Before I placed my finger on the chip, the temp count was about 32900. After placing my finger on the chip for at least 1 minute, the temp count increased to about 33250.
    <iframe width="480" height="270" src="https://youtube.com/embed/qg8uyCFrL_Y" allowfullscreen> </iframe>

5. I ran the `Example1_MicrophoneOutput` example to test the microphone. When playing a frequency of 250 Hz, the Serial Monitor shows the loudest frequency as 251 Hz. Playing 500 Hz shows the loudest frequency as 503 Hz.
    <iframe width="480" height="270" src="https://youtube.com/embed/9jgw84aqkiE" allowfullscreen> </iframe>

<h4>Additional Tasks for 5000-Level Students</h4>
1. Using Wikipedia's [Piano key frequencies](https://en.wikipedia.org/wiki/Piano_key_frequencies) article, I chose a' (440 Hz), a'' (880 Hz), and a''' (1760 Hz). In a new sketch, I copied the code from `Example1_MicrophoneOutput`, commented out Line 124 and added the following code after that line. If the frequency played is +/- 10 Hz of the desired musical note, display the corresponding musical note. Otherwise, the Serial Monitor displays "Frequency out of range".

```cpp
    if (ui32LoudestFrequency >= 430 && ui32LoudestFrequency <= 450) {
        // 440 Hz
        Serial.println("Piano musical note: a'");
    }
    else if (ui32LoudestFrequency >= 870 && ui32LoudestFrequency <= 890) {
        // 880 Hz
        Serial.println("Piano musical note: a''");
    }
    else if (ui32LoudestFrequency >= 1750 && ui32LoudestFrequency <= 1770) {
        // 1760 Hz
        Serial.println("Piano musical note: a'''");
    }
    else {
        Serial.println("Frequency out of range");
    }
```

<iframe width="480" height="270" src="https://youtube.com/embed/E5qhQpawyok" allowfullscreen> </iframe>

<div style="margin-top: 20px;"></div>
### <u>Lab 1B</u>
<div style="margin-top: 20px;"></div>
<h4>Prelab</h4>
I installed Python 3.13 and pip. I created a new virtual environment using Command Prompt and installed the necessary Python packages. I installed the codebase into my project directory. I installed the ArduinoBLE library and uploaded `ble_arduino.ino` to the Artemis board. I confirmed that the Artemis board is printing its MAC address. 

<h4>Configurations</h4>
1. In connections.yaml, I updated the MAC address to `artemis_address: 'c0:81:c4:21:22:64'`, using the address printed in Serial Monitor.
    <figure> <img src="{{ '/assets/images/Lab1B_Configs1.jpg' | relative_url }} width="480">
    </figure>

2. In a new Jupyter notebook, I ran the following code to generate a new UUID.
    <figure>
        <img src="{{ '/assets/images/Lab1B_Configs2.jpg' | relative_url }}"
            width="480">
    </figure>
    In `ble_arduino.ino`, I replaced the BLEService UUID with the generated UUID.
    <figure>
        <img src="{{ '/assets/images/Lab1B_Configs2_2.jpg' | relative_url }}"
            width="480">
    </figure>
    In `connections.yaml`, I replaced ble_service with the generated UUID.
    <figure>
        <img src="{{ '/assets/images/Lab1B_Configs2_3.jpg' | relative_url }}"
            width="480">
    </figure>

3. I checked the UUIDs in `ble_arduino.ino` and `connections.yaml` to make sure they are the same.
    <figure>
        <img src="{{ '/assets/images/Lab1B_Configs3.jpg' | relative_url }}"
            width="480">
    </figure>
    <figure>
        <img src="{{ '/assets/images/Lab1B_Configs2_3.jpg' | relative_url }}"
            width="480">
    </figure>

4. I checked that the command types defined in `enum CommandTypes` in `ble_arduino.ino` are the same as the ones defined in `cmd_types.py`.
    <figure>
        <img src="{{ '/assets/images/Lab1B_Configs4.jpg' | relative_url }}"
            width="480">
    </figure>
    <figure>
        <img src="{{ '/assets/images/Lab1B_Configs4_2.jpg' | relative_url }}"
            width="480">
    </figure>

5. I uploaded `ble_arduino.ino` to the Artemis board.

6. I ran all of the code blocks in `demo.ipynb`. This is the output displayed in the Serial Monitor. 
    <figure>
        <img src="{{ '/assets/images/Lab1B_Configs6.jpg' | relative_url }}"
            width="480">
    </figure>

<h4>Tasks</h4>
1. In case `ECHO`, I added the following code. 
    <figure><img src="{{ '/assets/images/Lab1B_Task1_1.jpg' | relative_url }}" width="480"></figure>
    In a Jupyter notebook, I ran the following code to send the string value `NI HAO-DY`.
    <figure><img src="{{ '/assets/images/Lab1B_Task1_2.jpg' | relative_url }}" width="480"></figure>
    The Artemis board received the string value and printed an augmented string to the Serial Monitor. 
    <figure><img src="{{ '/assets/images/Lab1B_Task1_3.jpg' | relative_url }}" width="480"></figure>

2. In case `SEND_THREE_FLOATS`, I wrote the following code.
    <figure><img src="{{ '/assets/images/Lab1B_Task2_1.jpg' | relative_url }}" width="480"></figure>
    In a Jupyter notebook, I ran the following code to send three floats, `6.74`, `4.25`, and `2.19`.
    <figure><img src="{{ '/assets/images/Lab1B_Task2_2.jpg' | relative_url }}" width="480"></figure>
    The Artemis board received the three floats and the values are printed in the Serial Monitor.
    <figure><img src="{{ '/assets/images/Lab1B_Task2_3.jpg' | relative_url }}" width="480"></figure>

3. To add the new command `GET_TIME_MILLIS` to class `CMD`, I modified `cmd_types.py` and `CommandTypes` in `ble_arduino.ino`.
    <figure><img src="{{ '/assets/images/Lab1B_Task3_1.jpg' | relative_url }}" width="480"></figure>
    <figure><img src="{{ '/assets/images/Lab1B_Task3_2.jpg' | relative_url }}" width="480"></figure>
    I created a new case for `GET_TIME_MILLIS` and wrote the following code.
    <figure><img src="{{ '/assets/images/Lab1B_Task3_3.jpg' | relative_url }}" width="480"></figure>
    In a Jupyter notebook, I ran the following code to make the robot output a time stamp to the string characteristic.
    <figure><img src="{{ '/assets/images/Lab1B_Task3_4.JPG' | relative_url }}" width="480"></figure>
    The Artemis board outputs the time to the Serial Monitor in milliseconds.
    <figure><img src="{{ '/assets/images/Lab1B_Task3_5.jpg' | relative_url }}" width="480"></figure> 

4. I wrote the following code to create a notification handler in a Jupyter notebook to retrieve the string value from the Artemis board. This string value contains the time stamp from the board.
    <figure><img src="{{ '/assets/images/Lab1B_Task4.jpg' | relative_url }}" width="480"></figure> 

5. I created a new command `GET_TIME_MILLIS_FIVE_SEC` by modifying `cmd_types.py` and `CommandTypes` in `ble_arduino.ino`.
    <figure><img src="{{ '/assets/images/Lab1B_Task5_1.jpg' | relative_url }}" width="480"></figure> 
    <figure><img src="{{ '/assets/images/Lab1B_Task5_2.jpg' | relative_url }}" width="480"></figure> 
    I created a new case `GET_TIME_MILLIS_FIVE_SEC` and wrote the following code.
    <figure><img src="{{ '/assets/images/Lab1B_Task5_3.jpg' | relative_url }}" width="480"></figure> 
    In Jupyter notebook, this code prints as many time stamps as possible in 5 seconds. The output is shown in the notebook instead of the Serial Monitor.
    <figure><img src="{{ '/assets/images/Lab1B_Task5_4.jpg' | relative_url }}" width="480"></figure> 
    In these 5 seconds, 213 messages totaling 4,473 bytes were received from the Artemis board. This is an effective 42.6 messages or <b>894.6 bytes per second</b>.

6. I created a new command `SEND_TIME_DATA` by modifying `cmd_types.py` and `CommandTypes` in `ble_arduino.ino`, similar to Tasks 3 and 5. I created an array `time_array` with a size of 300 in the global variables.
    ```cpp
        const int array_length = 300;
        unsigned long time_array[array_length];
    ```
    I created a new case and wrote the following code. The time stamps are stored into `time_array` for up to 5 seconds or until the array is full. After 5 seconds or the array is full, the notification handler receives each data point in the array as a string. 
    <figure><img src="{{ '/assets/images/Lab1B_Task6_1.jpg' | relative_url }}" width="480"></figure> 
    In Jupyter notebook, this code displays all of the time stamps in the array. 
    <figure><img src="{{ '/assets/images/Lab1B_Task6_2.jpg' | relative_url }}" width="480"></figure>
    The array is full after 11 ms and stored 300 time stamps totaling 6,300 bytes. This is an effective rate of <b>572,727 bytes (or 0.573 megabytes) per second.</b> 

7. I created a new command `GET_TEMP_READINGS` by modifying `cmd_types.py` and `CommandTypes` in `ble_arduino.ino`, similar to Tasks 3 and 5. I created a new array `temp_array` with the same size of 300. 
    ```cpp
        unsigned long temp_array[array_length];
    ```
    I created a new case and wrote the following code. Similar to Task 6, the temperature readings are stored into `temp_array` for up to 5 seconds or until the array is full. 
    <figure><img src="{{ '/assets/images/Lab1B_Task7_1.jpg' | relative_url }}" width="480"></figure>
    I created a new notification handler to parse through the string and add the information into two dedicated lists for time and temperature.
    <figure><img src="{{ '/assets/images/Lab1B_Task7_2.jpg' | relative_url }}" width="480"></figure>
    The array is full after 117 ms and stored 300 time stamps and temperature readings totaling 15,000 bytes. This is an effective rate of <b>128,205 bytes per second</b>.

8. The second method involving adding information to an array offers a much greater data transfer rate than sending data every loop iteration. The advantage of the first method (Task 5) is that it allows us to see the data in real-time, but a disadvantage is a much slower data transfer rate. The advantage of the second method (Tasks 6 and 7) is less overhead which allows higher transfer rates, but a disadvantage is that this method is RAM intensive. The first method is best for live monitoring of low-frequency signals, the second method is best for lab equipment that requires high sampling rates.  
    <div style="margin-top: 10px;"></div>
    The second method recorded about 573 kB/s in Task 6 and about 128 kB/s in Task 7. With a RAM of 384 kB, this means you can only record 0.67 seconds in Task 6 or 3 seconds of data in Task 7. 
    
<h4>Additional Tasks for 5000-Level Students</h4>
1. 

<div style="margin-top: 20px;"></div>
<h3>Discussion</h3>
This lab shows how we can receive time-stamped messages from the Artemis board. The notification handler allows us to see the outputs in Jupyter notebook instead of using Serial Monitor in the Arduino IDE. One thing I noticed is that if a variable is defined in a case, the entire case has to be enclosed in braces for the code to compile.

<div style="margin-top: 20px;"></div>
<h3>Collaborators</h3>
I collaborated with Sam Zhen while working on this lab. I also looked at Selena Yao's 2025 website and Angela Voo's 2025 website as references.
    
        

   
