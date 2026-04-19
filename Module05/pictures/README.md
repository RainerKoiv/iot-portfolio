# Task 1 - Creating Your First IoT System

Pressing the button toggles the value pressed / released which can toggle the LED on / off.

![alt text](task1-video.gif)

The Node-Red flow:

![alt text](task1-flow.png)


# Task 2 - Second Node

Pressing the button on one node toggles the value pressed / released which can toggle the LED on / off on the second node.

![alt text](task2-video.gif)

The Node-Red flow:

![alt text](task2-flow.png)


# Task 3 - rebuild access control system with IoTempower (4+ nodes)

Using two D1 Minis and one ESP32, we connected a RFID reader, a buzzer and an OLED screen to display "granted" and make high pitched noise, if access is granted.

![alt text](task3-video.gif)

The Node-Red flow:

![alt text](task3-flow.png)


# Hostel fire scenario

When CO2 level goes above 2000 and temperature above 36°C, then:
* the buzzer will start making noise
* the motor will turn 90° to simulate doors closing
* the distance sensor will start detecting if something is in the 20 cm range. If there is movement, then the motor will turn to 0° to simulate doors opening for the person.

![alt text](hostel-fire-scenario.gif)

The Node-Red flow:

![alt text](module5-hostel-fire-scenario-flow.png)