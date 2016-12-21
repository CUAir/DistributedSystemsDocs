.. CUAir Distributed Systems Documentation documentation master file, created by
   sphinx-quickstart on Mon May  2 11:28:43 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


Test Flight
============================

.. contents::

This section outlines the distributed systems test flight procedure.

192.168.0.21 - NUC is static IP
192.168.0.50 - attena tracker

Pre-arrival Checklist
-----------------------
* Make sure you build most recent version of ground-test before getting to the field (no internet), **NEVER EXIT ACTIVATOR SHELL**
* Bring Targets (Shapes and Alphanumerics)

Ground Setup Checklist
------------------------
1. Setup all the targets in the field and record gps lat/long and orientation coordinates
2. Open compass app and take screen shot
3. Make sure that you restart compass app when you go to new target location
4. Make sure database is empty (run a test in the activator shell)
5. Do “run” in activator shell
6. Turn on nuc, gimbal, and comms (on left side, down is on)
7. Turn on button on top left of dashboard (should turn green)
8. Take off lens cap from camera
9. Set focus of camera on plane to infinity
10. Make sure antenna tracker is on
11. Set a static IP address for yourself

12. Open 4 other terminals

  a) Start Gimbal server:

  ::

     ssh@192.168.0.21
     cd odysseus/gimbal-server
     ./run.py -a 8001

  b) Start Camera server:

  ::

     ssh@192.168.0.21
     cd odysseus/camera-server
     ./app.py -dh <your-static-ip> -dp 9000

  c) Start Plane Autopilot server:

  ::

     ssh@192.168.0.21
     cd MAVProxy2/MAVProxy
     source venv/bin/activate
     python mavproxy.py —-master=/dev/ttyUSB0 57600

  * Try USB1 and USB2
  * To see which one is communicating data, do a cat on any of those USB files
  * DON’T TYPE ANYTHING

13. Access Database

::

  sql -U postgres plaedalus

14. Extra Terminal for pinging NUC (192.168.0.21)
  a) If connection to NUC ever goes down, restart all above servers

Ground Pre-Flight Test
------------------------
1. Open localhost:9000 (MDLC)
2. Test gimbal - point at ground and retract

  a) Make sure you enter numbers in for point-at-gps coordinates or the request will fail
  b) Ensure that the gimbal rotates and the motor works
  c) Check in the database (select * from gimbal_settings) to see when your request goes from queued to sent

3. Test camera

  a) Start taking pictures
  b) Stop taking pictures
  c) Check in database

    i. ``Select * from camera_settings;``

      1. Check that requests are getting sent

    ii. ``Select * from images;``

      1. Check that we are receiving images
      2. Check that they have a gimbal_id and telemetry_id (autopilot)

4. Ask for notification when about to land (to retract gimbal)

Ground Test Flight
-------------------

1. A couple seconds before take off, start taking pictures
2. Once plane takes off, set gimbal to point at ground
3. When they’re about to land, retract gimbal
4. Stop taking pictures

Post Flight
---------------
1. Stop ground server ONCE all images are received using ``CTRL D``

2. Dump database

``pg_dump -U postgres plaedalus > 05_11_2016_1.sql``

3. Move all images to pictures file
4. Create ``<date_testflight#>`` folder with

  a) Folder of all pictures
  b) Sql file
  c) Logs file

Debugging
-----------

1. Trying to ssh into NUC

  a) Possible error: Connection refused

    i. NUC is still powering on

  b) Possible error: No route to host

    i. Switch connecting you to the LAN is down

2. Can do “-h” for running anything and it’ll specify parameters


For Connecting Through Ethernet
-------------------------------
1. Connect to NUC through Ethernet
2. Change your local network settings to the following:

  a) IP Address: 192.168.1.26
  b) Subnet Mask: 255.255.255.0
  c) Router: 192.168.0.1

3. In Terminal, type ``ssh cuair@192.168.0.21``. The password is ``aeolus``. You should be able to access and run any programs on the NUC.
4. To get all the pictures captured by the NUC, go into another Terminal window and type: ``scp "cuair@192.168.0.21:~/odysseus/camera-server/*.jpg” ~/<path to the folder you want to put the pictures in>``

  a) Make sure to keep the quotation marks where they are so that the regex works
