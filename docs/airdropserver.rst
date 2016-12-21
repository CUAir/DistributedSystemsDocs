.. CUAir Distributed Systems Documentation documentation master file, created by
   sphinx-quickstart on Mon May  2 11:28:43 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


Airdrop Server
============================

.. contents::

This section provides the use and design of the distributed systems airdrop server.

Overview
----------------

The CUAir airdrop server is responsible for delivering a care package as part of the search-and-rescue mission. As such, the airdrop server is constantly running and communicating with both the ground station server and the autopilot server.

**Full documentation of the 2015-2016 Airdrop Server API can be found** `here <http://docs.cuair20152016airdrop.apiary.io/>`_.

The airdrop server is built using the Flask web framework in Python.

Airdrop System Description
----------------------------

When the airdrop server is started, two jobs are added to a scheduler - **update and drop**. The drop job is scheduled, initially, to execute three hours in the future. The update job is set to execute at an interval, every 0.05 seconds. The update job reads the plane’s telemetry data (GPS location/altitude), and if the most recent setting is in the correct mode (discussed below), performs calculations that determine the amount of time until the payload should be dropped. The update job then schedules the drop job to drop the care package at that time in the future.

From the ground station, a user saves a setting through the front end, which is saved in a Settings table. When the user saves a setting, he/she specifies the mode, the location of the target, and the threshold.

The **threshold** is how close the package should be dropped to the target in feet - this value is generally set lower than the threshold specified in the rules.

There are three different modes that the user can set. When the server is in default mode, “not armed”, the package is never dropped. This mode is necessary to both save the location of the target and to make sure that we notify the judges before dropping the payload. In this mode, the update job which runs continuously does not query the autopilot server for telemetry data or perform any mathematical computations. The second mode, “armed” is set so that the server will drop the package if it is in the threshold. Now, in the update job, airdrop queries the autopilot server for telemetry information and performs calculations using this information, and the setting information. After calculations, the drop job is rescheduled with the updated time to drop.

The third mode, override, schedules the drop job to execute at the current time. The drop job, when executed, communicates with the airdrop board and to set the angles of the mechanism to “open”, which drops the payload.

Also see the presentation give on Airdrop `here <https://docs.google.com/presentation/d/1bkWt93OvCaJGoPV4hLBj9kzwlWbTHkdznSIj07GO1pI/edit/>`_

Connecting to the NUC
-------------------------

Turning the NUC ON
^^^^^^^^^^^^^^^^^^^^^
What you need:

* Big Black Block Power Supply or Battery that can plug into the plug mentioned below

1. Connect the Big Black Block Power Supply/Battery to the plug on the left side of the blue board (control panel) on the plane
2. Turn the power supply on, make sure it’s connected to electric socket.
3. Make sure Rocket AC (white rectangular thing) is blue (powered).
4. Then switch Comms and NUC to down.
5. Then hit the top button of the 2 push buttons on the blue board and make sure it’s green.

  a) This turns on the NUC!

Connecting to the NUC
^^^^^^^^^^^^^^^^^^^^^^^^

(FOLLOW DIRECTIONS IN ORDER)

What you need (If you don’t know where any of these boxes are just ask around):

* Another Rocket AC (white rectangular thing) it’s a router

  * Antenna Tracker box

* 2 antennas (don’t have to be same frequency

  * Antenna Tracker box

* POE connector (has 2 ethernet ports, 1 is named POE) has name “power active”

  * In ground station box

* Ethernet cables

  * In ground station box

* Ethernet to USB converters

  * In ground station box


1. Put antennas on the rocket AC (do this first)
2. Connect rocket AC to POE port (NOT LAN port) of the POE connector via ethernet cable
3. Then plug the LAN port to your computer via ethernet
4. Ethernet to USB converters for MAC in the ground station box.

**Make sure you undo everything that you did and put everything back into the right boxes!**

SSH into NUC
^^^^^^^^^^^^^^^
See the "For Connecting Through Ethernet" section of "Test Flight".

Other Notes
^^^^^^^^^^^

With the NUC and comms currently integrated in the fuselage, it’s no longer convenient to hook up a monitor and keyboard to the NUC to enable DHCP so that the NUC can connect to the internet. To make things easier, the em1 interface is now permanently set to a static ip (192.168.0.21) and connected to the AC Wi-Fi comms system so that any computer connected to the ground station rocket AC can SSH into the NUC. A new eth0 interface has been added with DHCP enabled so if an ethernet cable with a usb-ethernet adapter is plugged into a usb port, the NUC will have access to the internet. This allows anyone SSH’d into the NUC to now also access the internet (and pull any relevant code). However, this requires the eth1 interface to be disabled, which is normally used for SRIC. As a result, before connecting to the internet, double check the interfaces file (/etc/network/interfaces) and make sure that the eth0 interface is enabled and eth1 interface is disabled and vice versa for SRIC.

tl;dr:
Connect to the rocket under the granite table via ethernet with a 192.168.0.x IP to talk to the NUC (192.168.0.21)

Change the interfaces file on the NUC (/etc/network/interfaces) to enable/disable DHCP (eth0 for internet, eth1 for SRIC)
