# Module 3

### Mango router (300M? N? Mini Smart Router)
* https://store.gl-inet.com/products/gl-mt300n-v2-mini-smart-router
* It’s a router, switch, wifi AP, DNSmasq DHCP server in one. Also has vpn server
* 4 modes: router, repeater, AP, hotspot
* Not a lot of power; can connect around 30 devices to the WiFi
* WiFi speed: 300Mbps (2.4GHz)
* ipv4; OS: OpenWRT (based on Linux)


## Connect to Maria’s Network 

Steps:
* Our hostname was Team10 (same as the router nr).
* We downloaded an MQTT client application - MyMQTT.
* Matias used his phone to connect to Maria's WiFi and on the MyMQTT app connected to MQTT broker.
* Rainer installed MQTT-Explorer on his computer.
* On the MyMQTT app, we subscribed to instructions/# and started waiting for further instructions.

## Phase 1 - Set up and configure your LAN

Q: The router has OpenWRT installed. Research what OpenWRT is. Take some notes in your portfolio.

A:
* The OpenWrt Project is a Linux operating system targeting embedded devices.
* Standardized, stable, free, well tested, for many routers
* Provides a fully writable filesystem with package management.
* Allows customization of the device through the use of packages to suit any application.

------------
Q: Research the difference between LAN and WAN. Do we always need an internet connection for networks? Take some notes in your portfolio.

A:
* LAN (Local Area Network) connects devices over a small, localized area like a home or office with high speeds.
* WAN (Wide Area Network) connects multiple LANs across large geographical areas, such as cities or countries, typically via an ISP.
* We do not always need an internet connection

-----------

Intranet vs Internet
* intranet (local) - An intranet is a private network that is contained within an organisation or place. It uses the same Internet Protocol (IP) standards and Transmission Control Protocol/Internet Protocol (TCP/IP) infrastructure as the public Internet, but its access is restricted to authorized users. An intranet can facilitate communication within an organization and share information and resources among employees.
* internet (global) - The Internet is a global network of computers that allows users to communicate with each other and access information from around the world. The Internet comprises millions of individual networks, each with its collection of computers and devices. Users can access the Internet from anywhere in the world using various devices, including personal computers, laptops, smartphones, and tablets. The Internet provides users with vast information and resources, including news, entertainment, education, businesses, and more.

-----------

Q: Research what DHCP/DNS servers are. Take notes on your portfolio.

A:
* DHCP (Dynamic Host Configuration Protocol) -  responsible for assigning dynamic IP addresses to devices within a network. It works in a centralized way, using the server-client model. The DHCP server dynamically and automatically provides IP addresses to every newly connected device.
* DNS (The Domain Name System) - converts human-readable domain names ( into machine-readable IP addresses, also known as DNS resolution. DNS works through a hierarchical structure consisting of root servers, Top-Level Domain (TLD) servers, Authoritative servers, and Recursive DNS servers. A computer sends a DNS query to a Recursive DNS server, which crosses the DNS hierarchy to find the corresponding IP address. The resolved IP address is cached to improve future lookups. Google.com → 8.8.8.8


-----------

Steps:

* Since there already was OpenWRT on the mango router, we didn’t need to flash it
* Connected a computer to the router LAN port using the Ethernet cable + powered it with an usb cable
* Using “ipconfig” we found the routers ipv4 address
* Typing the router’s IP address as the URL, we opened the router’s LuCI interface in a browser on our computer that was connected to the router
* In the header, selected network → interface and edited the LAN IPv4 address to 192.168.14.1.
* We disconnected and reconnected from the computer by unplugging the Ethernet cable and replugging it into the computer, so that we can use the new IP
* Logged into the router using the new IP address

[Phase 1 images](https://github.com/RainerKoiv/iot-portfolio/tree/main/Module03/pictures#phase-1)


## Phase 2 - Set up and configure your WiFi Accesspoint

Q: Research about 2.4 GHz Wi-Fi band and the European (ETSI) standards. Research what WiFi channels are and understand why it is important to choose a less busy one. Can all channels be used in Estonia? 

A:
* GHz Wi-Fi band - provides the most coverage but transmits data at slower speeds.
* The European (ETSI) standards - produces globally applicable standards for Information and Communication Technologies (ICT), including mobile (GSM, 3G, 4G, 5G), fixed, radio, broadcast, and internet technologies.
* WiFi channels - 14 channels are designated in the 2.4 GHz range, spaced 5 MHz apart from each other except for a 12 MHz space before channel 14.
* Important to choose a less busy one to reduce signal interference from neighboring networks and devices.
* No, channels 1–13 can be used in Estonia.

-----------

Steps:

- Using the Channel analysis tool in OpenWRT, we found a less-busy WiFi channel. In our case it was 1
- Under network → wireless, we set up a new WiFi Access point with the SSID Team10
- After ensuring the network is secure with WPA2-PSK encryption, we set the WiFi password “iotempire”
- Changed the channel to the previously found less-busy channel. Disabled the existing OpenWrt WiFi access point.

[Phase 2 images](https://github.com/RainerKoiv/iot-portfolio/tree/main/Module03/pictures#phase-2)


## Phase 3 - MQTT Broker set up

Steps:
* Connect the router to an internet point (switch) via Ethernet to the WAN port on the router. You can connect the computer to the LAN via your new WiFi (Team10) now.
* Tested if we have internet connection on the router by going under network → diagnostic. Pinging openwrt.org we saw that packages are transmitted, meaning that we had internet on the router.
* Under System → Software, updated the List of packages available, then installed the following packages: 
  * mosquitto-ssl
  * luci-app-mosquitto
  * luci-app-commands 
* Configured the Mosquitto Broker under the Mosquitto tab:
  * Checked the box “Use this LuCI configuration”.
  * Enabled “Allow anonymous connections” field.
  * Added new Listener, configured it by setting the Port to 1883
  * Under Protocol dropdown, set it to listen to MQTT.
  * System -> Custom Commands -> Configure. 
  * Added new services, name it Running Services and type "netstat -tulpn" on the command box. 
  * Go back to the Custom Commands → Dashboard and run the command. 
  * Under PID/Program name, saw Mosquitto running.
 
[Phase 3 images](https://github.com/RainerKoiv/iot-portfolio/tree/main/Module03/pictures#phase-3)


## Phase 4 - Test MQTT messaging using devices

Q: Research how topics and publish/subscribe communication work in MQTT. 

A: 
* MQTT (Message Queuing Telemetry Transport) is a standards-based messaging protocol, or set of rules, used for machine-to-machine communication. Smart sensors, wearables, and other Internet of Things (IoT) devices typically have to transmit and receive data over a resource-constrained network with limited bandwidth. These IoT devices use MQTT for data transmission, as it is easy to implement and can communicate IoT data efficiently. MQTT supports messaging between devices to the cloud and the cloud to the device.
* MQTT works through a publish/subscribe model where clients connect to a central broker. Publishers send messages to specific topics, and subscribers receive messages by subscribing to those topics. 
* Example: slack, or sensor publishes data to temp topic, device can subscribe to that topic.

-----------


Steps:
* Connected our phones to the network's WiFi
* In the MQTT client app, connected to the MQTT broker.
* Created 2 topics with a subtopic for testing. For example: team10/mqtt/test. With each team member subscribing to both topics. 
* Tested all 2 topics by sending messages back and forth between phones. 
* Both published a message on both topics. Ensuring messages are successfully sent and received across different devices.

-----------

Q: Research about wildcards in MQTT and how they work. What does the # listen to. How do the topic and subtopic work and their hierarchy? Take notes in your portfolio. 

A:

In MQTT, wildcards provide a powerful mechanism for subscribing to multiple topics simultaneously. There are also subtopics, which are topics within topics.

Wildcard # - listen to all topics, example Delta/# - get all other topics in Delta

The + wildcard matches exactly one level in the hierarchy, like home/+/temperature. examples: 
- home/livingroom/temperature
- home/kitchen/temperature
- home/bedroom/temperature

Steps:
* Connected the computer to the router via Ethernet, opened MQTT-Explorer and connect to the MQTT broker. 
* Subscribed to all topics using the wildcard.
* Sent messages using your phone and checked if the computer can see all of them.

[Phase 4 images](https://github.com/RainerKoiv/iot-portfolio/tree/main/Module03/pictures#phase-4)

## Tutorial for setting up IoT network on a router (just do what we did 🥀)

1. Connect a computer to the router LAN port using the Ethernet cable + power it with an usb cable
2. open the router’s LUCI interface in a browser on your computer that is connected to the router
3. set up a new WiFi Access point with a SSID and find a less-busy WiFi channel
4. set the network with WPA2-PSK encryption and password
5. update the List of packages available, then install the following packages: 
   * mosquitto-ssl
   * luci-app-mosquitto
   * luci-app-commands 
6. Configure your Mosquitto Broker under the Mosquitto tab:
   * Check the box “Use this LuCI configuration”.
   * Set to Enable “Allow anonymous connections” field.
   * Add a new Listener, and configure it by setting the Port to 1883
   * Under Protocol dropdown, set it to listen to MQTT.
7. If luci-app-commands was correctly installed, you could check if the services that are running on the dashboard in the Mosquitto tab.
   * Go to System -> Custom Commands -> Configure. 
   * Add new services, name it Running Services and type "netstat -tulpn" on the command box. 
   * Go back to the Custom Commands → Dashboard and run the command. 
8. Test out MQTT apps and communications


## Reflection 3
[Reflection 3](/Reflections/ref03.md)

## Week 2: Phase Extra - USB Tethering for Internet access

Steps:
* Powered Mango with 12V → 4x5V USB charger, connected Mango router to WAN
* Went to router’s config and from the software tab installed kmod-usb-net-rndis
* Connected phone via USB and activated tethering
* On router’s config, went to network → interfaces → Add new interface
* Named it usb1, set the device to eth1, under firewall settings assigned the firewall-zone to wan wan6
* Clicked Save & apply, rebooted the router, disconnected WAN, had internet on the laptop that was connected to the WiFi

To start the USB tethering, you need to activate it on your phone and connect the phone to the router. Then configure the router to use the phone internet as WAN and then you can connect other devices to the router to access the Internet.

[Phone connected to the router]()

## First Aid Station Scenario Play

For this we (router 10 / Team10) teamed up with people with routers 18 and 15.

Team 15 worked on setting up the button and MQTT broker: https://github.com/RedToxyl/thw-iot/tree/main/Module03

Team10 and 18 worked on the temperature sensor, which was difficult to set up because of some errors when trying to run the code.

Steps:
* Used Arduino IDE, installed the necessary MQTT and Dallas Temperature libraries. 
* Combined the example codes to receive and output temp data and published it to the topic station/supplies/blood/temp

We completed all 4 scenarios. Some images are on team 15’s GitHub: https://github.com/RedToxyl/thw-iot/tree/main/Module03/pictures

Scene 1 result - Dr Anya saw temps rising\
Scene 2 result - Gauze request and confirmation received in broker\
Scene 3 result - Anya received code blue alert by being subscribed to station/alert/#\
Scene 4 result - Anya's broadcast received by others

[Scene results]()

## Iotempower

Steps:
* Installed it on our Linux / WSL with ``curl -L https://now.iotempower.us | bash -``
* Ran iot and then used ``mosquitto_sub`` command to listen to temperatures
* Used ``mosquitto_pub`` command to publish a message to station/announcements and saw it on our phones

[IoTempower]()

## MQTT ON NODE-RED

Steps:
* Started it with: ``iot service start --web`` and went to ``http://localhost:40080``
* Logged in, used nodes MQTT IN and DEBUG to listen to temperatures
* Not sure if team 15 managed to do the buttons, but team 18 created the gauge for temperatures as well.
* Ran out of time to do threshold warning and RGB LED

[MQTT on NODE-RED]()

## Reflection 4
[Reflection 4](/Reflections/ref04.md)


