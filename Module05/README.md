# Module 5

## Scaling IoT Systems (notes)

1. As of now, how do we create our IoT system (use the access control project as example)? Mango, Arduino IDE, etc?
- Intro. Nodes - breadboards, wires, connecting components. Networking - router mango, ethernet cables, wifi, MQTT protocol. Software / programming - arduino IDE / PlatformIO. Integration - Node-Red. Module 5 - to connect all, we need IoT Systems. We set up the Mango router, we write code in PlatformIO, we flash code to microcontrollers, we collect info from other iot devices in the broker, we use the broker to collect and send data to other software.

2. What is difficult to manage? Pain points?
- Node-red, flashing, debugging and fixing the problems (with iot devices / code) take a long time.

3. Can it scale? Hundred devices? Different AccessPoint/MQTT broker?
- Yes, there might be 100,000 devices in some places, this would require multiple access points since many can handle only 200 devices.


### Problems when scaling …

- ... scale developing and flashing MCUs
  - one by one - no; having script will be helpful 
  - flashing bottleneck - flash all of them over the air
- ... scale and management one big system
  - privacy and security - not a lot of memory on the devices
  - wifi packages colliding, the range - bluetooth, LoRa
- ... scale managing several systems
  - many brokers will need to communicate with each other, so building a connected network of brokers is necessary
  - mango can connect limited devices
- ... scale testing
  - more devices - more difficult to test - use scripts to track
  - making a test with node red to do pre-tests will be needed - simulations
- Which services would you like to see for scaling up?
  - iot cloud solutions
  - logging systems
  - UI-s

### Device management and integration for IoT Systems

Strong points from each:

Device Mgmt. Frameworks:
* ESPHome - free, open source, we have many usb devices
* Tasmota - Lightweight, open source, easy to flash?
* Mongoose - open source
* IoTempower - open source, what we use

Integration Frameworks:
* IoTKnit - simple way to simulate testing for IoT networks
* Node-RED - using a node style programming can make it easy to set up different integrations between devices
* Home Assistant - makes it simple to control all devices in a single app
* OpenHAB - Free of cost, Freedom from insecure cloud services if desired, flexible, powerful, can be run on a low power computer such as a Raspberry Pi, No vendor lock in.
* NiFI - provides a graphical user interface for creating, monitoring, and managing data flow
* Flogo - open-source, ultra-lightweight integration engine designed for edge, microservices, and serverless applications

What is the difference between integration and Mgmt Framework:
* IoT integration frameworks focus on connecting disparate devices, protocols, and data sources (IT/OT convergence) to enable interoperability, whereas management frameworks focus on the ongoing lifecycle administration, security, configuration, and orchestration of those IoT systems. Integration is about interoperability; management is about control. 

## Task 1 - Creating Your First IoT System

Following the IoTempower tutorial we set up the first node.
- we already had the iot installed
- on the router we installed mosquitto-nossl and luci-app-mosquitto packages
- the permissions were enabled in our previous labs
- ‘iot x web_starter’ did not work so we used ‘iot service start --web’
- Made the template system
- changed the config, to match our router and MQTT
- edited the setup file and flashed the esp8266
- got a working result

The code in Node1 setup.cpp:

```
// Button on pin D3
// Change debounce from 5 to 10 milliseconds
input(button1, D3, "released", "pressed").debounce(10);

// Onboard LED (note: inverted for most ESP boards)
output(blue_led, ONBOARDLED).inverted();
```

[Node1 Node-Red flow and result](https://github.com/RainerKoiv/iot-portfolio/tree/main/Module05/pictures#task-1---creating-your-first-iot-system)

[exported flow](flows/task1-flow.md)

## Task 2 - Second Node

Did the same as task 1, but made it so, that the MQTT is sending to the other node to turn the LED on and off.

The code in Node2 setup.cpp:

```
input(b1, D3, "up", "down");

// Onboard LED (note: inverted for most ESP boards)
output(blue, ONBOARDLED).inverted();
```

[Node2 Node-Red flow and result](https://github.com/RainerKoiv/iot-portfolio/tree/main/Module05/pictures#task-2---second-node)

[exported flow](flows/task2-flow.md)

## Task 3:  rebuild access control system with IoTempower (4+ nodes)

We had 3 boards (2 D1 Minis and 1 ESP32) and 3 USB cables. We used three nodes (one for each board) because it already took us a lot of time to get it working so trying to use multiple sensors on a single board would have been more time consuming and difficult. Spent a lot of time trying to get the RGB LED to work, but in the end didn't manage.

Story:
```
Scenario Description
====================

title: Access control system lite-version

When a user swipes their keycard on the RFID reader, the OLED display will show a msg and the buzzer will make a noise.
If access is granted, then the OLED displays "granted" and the buzzer will make one sound.
If access is denied, then the OLED displays "denied" and the buzzer will make another sound.
```

Node-RED:

The MQTT IN reads the uid of the cards and if there is none it ignores it. The function node checks for the allowed cards and sends ‘denied’/’granted’ depending on the allowed list. For the OLED screen we send the result to it to show ‘denied’/’granted’ on the screen and then we clear it 1 second later. For the buzzer we change the frequency to 512/1000 depending on ‘denied’/’granted’. We also set the duty frequency time and set it to 0 after 1 second


RFID-reader code:

```
mfrc522(reader);
```

Buzzer code:

```
pwm(buzzer, D5/*, frequency is by default 1000*/); /* replace the full comment with , 511 if you want to change the def>
```

Display code:

```
// default ssd1306 128x64 i2c display
//display(dp1).i2c(D4, D3).with_address(0x78);

// small Wemos D1 Mini SSD1306 64x48 display shield
U8G2_SSD1306_64X48_ER_F_HW_I2C u8g2(U8G2_R0); // R0 no rotation, R1 - 90°
display(oled, u8g2, font_tiny);

// Relatively large two line display in many sensor-kits
//display44780(display2, 16, 2);
```


[access control system Node-Red flow and result](https://github.com/RainerKoiv/iot-portfolio/tree/main/Module05/pictures#task-3---rebuild-access-control-system-with-iotempower-4-nodes)

[exported flow](flows/task3-flow.md)


## Reflection 8

[Ref08](/Reflections/ref08.md)


# Week 2 - Hostel HostmeWell Automation

We, team 10, teamed up with team 18.

## 3 Challenges

### Challenge 1: smoke detector system

Javier is a professional alarmer. During a fire he starts shouting and closes all the doors to prevent airflow. The problem is that the hostel is quite big and getting around takes time. During an emergency it is important to notify everyone as soon as possible and take action to put out the fire. All of this can be automated and made safer by using:
* a CO2 sensor (with temp sensor) for measuring air quality;
* a buzzer sensor for the alarm;
* a motor to close all the doors to block airflow;
* a distance sensor for the doors. When the doors are closed during a fire, they will automatically open when a person comes near them. 
We will be using:
* our Mango router as the hostel network; 
* the MQTT Explorer for sending and receiving messages; 
* IoTempower for creating the system and the nodes for the iot sensors; 
* Node-Red for the flow modelling.
Difficulties: setting up the new motor device; complex Node-Red flow


### Challenge 2: access control with automatic lights

Elina and Markus have come to the hostel after a 20h flight, it is 23:00 and pitch black outside. Once they get to the room all they wanna do is go to sleep. To help with that the room’s lights automatically light up when the door is opened and shut off if the sensors don’t detect any movement in the room.

How it works:
1. Elina swipes her RFID card at the room door.
2. The system verifies her credentials via the access control software.
3. If authorized:
  * The smart lock opens.
  * The lighting system is triggered ON.
4. Motion sensors continue to monitor occupancy.
5. If no motion is detected for a predefined period (e.g., 10 minutes), lights automatically turn OFF.

Components:
* RFID card readers (for employee identification)
* Smart locks (electronically controlled doors)
* Buzzer for notifying that the door is unlocked
* led for notifying if the door is unlocked or locked
* Motion sensors (detect presence in a room)
* LED strip (control lighting circuits)
* IoT platform (e.g., device communication and automation logic)

Difficulties: What counts as motion? Only the floor or bed as well?

### Challenge 3: "A Cleaner’s Dilemma Solved by a Silent Guardian"

Characters: Eva - diligent cleaner at hostel hostmewell

Eva starts her shift at 7am. She is a cleaner at the hostel. Guests often forget to hang up the “Do not disturb” sign at the door, which means that Eva has to knock on everyone's doors and ask for housekeeping. That makes for some embarrassing situations - if no one answers the door, Eva assumes that the room is empty, but sometimes the guest is just a heavy sleeper.

The Solution: Each bed has a small pressure/weight sensor (FSR or load cell) under the mattress connected to an D1 mini node. When no weight is detected for more than 30 minutes after 6am, the system flags the bed as "likely vacated." A Node-RED flow sends a notification to the cleaning staff's dashboard and logs the time.

Hardware: FSR pressure sensors, D1 mini, MQTT broker (IoTempower)

Software: IoTempower, Node-RED dashboard, MQTT with a debounce/delay filter to avoid false positives from guests tossing in their sleep

Filter used: Time-based filter/debounce that only triggers after sustained no-pressure signal

Difficulties: Sensor calibration per bed weight, false positives from luggage left on beds, privacy concerns with continuous monitoring

## Prototyping

We chose challenge 1: smoke detector system

Buses used for the sensors:
* CO2 sensor - I2C
* distance sensor - Digital GPIO
* buzzer - Digital GPIO
* motor - PWM

Difficulties you found about certain sensors:
* general debugging steps of sensors not working like they were supposed to.
* The motor was a bit broken. It did not always work as expected, occasionally moving more or less than specified.

How to use the filter we have chosen:
* We chose a filter for the distance sensor, because we were only interested in movement near the door. In our case, we used a custom filter and set it to send the measured distance only if it’s under 200 mm. Maybe we could also have used one of the IoTempower predefined filters.

Unfortunately we didn’t have time to make dashboards or DIY models.

[Hostel fire scenario Node-Red flow and result](https://github.com/RainerKoiv/iot-portfolio/tree/main/Module05/pictures#Hostel-fire-scenario)

[exported flow](flows/module5-hostel-fire-scenario-node-red.md)

### Code

IoTEmpire code for CO2 sensor: 

```
scd4x(gas, 5.0, 60, 101300).i2c(D7, D5);
```

Code for distance:

```
hcsr04(distance, D5, D6)
    .filter([](Device& dev) {
        int val = dev.read_int();
        return val < 200;
    });
```

IoTEmpire code for buzzer:

```
pwm(bzz, D5, 2000);
```

IoTEmpire code for servo (motor):

```
servo(door, D5);
```

## Reflection 9

[Ref09](/Reflections/ref09.md)