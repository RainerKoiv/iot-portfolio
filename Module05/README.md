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

[Node1 Node-Red flow and result]INSERT LINK

[exported flow](flows/task1-flow.md)

## Task 2 - Second Node

Did the same as task 1, but made it so, that the MQTT is sending to the other node to turn the LED on and off.

The code in Node2 setup.cpp:

```
input(b1, D3, "up", "down");

// Onboard LED (note: inverted for most ESP boards)
output(blue, ONBOARDLED).inverted();
```

[Node2 Node-Red flow and result]INSERT LINK

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


[access control system Node-Red flow and result]INSERT LINK

[exported flow](flows/task3-flow.md)