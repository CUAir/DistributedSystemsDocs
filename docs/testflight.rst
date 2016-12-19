Test Flights
============

.. contents::

Overview
---------

The autopilot sub-team has developed numerous resources and strategies to be most efficient and productive at test flights. This page details these methods and consolidates links to essential resources. 

Preparation for a Test Flight
-----------------------------

Please read the `ArduPlane Documentation <http://ardupilot.org/plane/docs/introduction.html>`_  to familiarize yourself with tuning methodologies.

`Here <https://pixhawk.org/users/status_leds>`_ you can also find a description of the meaning of different Pixhawk status LEDs.


Before the Test Flight
-----------------------

`Charging batteries <https://docs.google.com/a/cornell.edu/document/d/1BB32SqGUB9Od7vRuGxLZDtUl3IxABGeZSRhjDGb9uEE/edit?usp=sharing>`_ -- Batteries need to be changed before every flight. This documentation describes how to do so safety. 


`Packing Checklist <https://docs.google.com/a/cornell.edu/document/d/1ayoTEOM1kUWVMDSlj4T_z2wIWLmFvlScaIrMWGEYeLU/edit?usp=sharing>`_ -- Before leaving for a test flight, test accordingly in the lab and ensure these items are packed.


At the Test Flight
-------------------

Roles -- Assigning more limited roles at test flights ensures team members can work concurrently. 

`Preflight Todo Checklist <https://docs.google.com/a/cornell.edu/document/d/1ayoTEOM1kUWVMDSlj4T_z2wIWLmFvlScaIrMWGEYeLU/edit?usp=sharing>`_ -- Follow these steps carefully to avoid common issues with flight

`Tuning guide <https://docs.google.com/a/cornell.edu/document/d/1GEGPoO7C8SVG3ce17zwZWjsApVifKiyOvoxnIkX4Or4/edit?usp=sharing>`_ -- This manual details the steps needed to tunning ArduPlane. Print the manual and follow along accordingly. 

`Flight Cards <https://docs.google.com/a/cornell.edu/presentation/d/1QKiTktPquDpCYcg-_-agLuD5t6N1zBHpDIT-jldJb_s/edit?usp=sharing>`_ -- Fill out one of these at each test flight.

Code to run at test flights
^^^^^^^^^^^^^^^^^^^^^^^^^^^
- `APM Planner <http://ardupilot.com/downloads/#apm_planner_20_9_raquo>`_ -- as a backup APM should be installed on all computers 

- `MAVProxy <http://cuairautopilot.readthedocs.io/en/latest/groundstation.html#ground-station>`_ -- Installation and instructions for using the ground station can be found here.

- `Interoperability <http://cuairautopilot.readthedocs.io/en/latest/groundstation.html#interoperability>`_ -- Setup can use of the interoperability server can be found here

--`Autopilot server <>`_ -- Setup and use of autopilot server for the NUC can be found here.


Hexacopter Setup
----------------

Atlas is a portable, lightweight test system to allow subteams to test without needing to do a full test flight. It is able to carry several pounds of payloads that can be mounted as necessary and is significantly easier to fly than a fixed wing aircraft. 

The base onboard electronics include:

- 1x `Pixhawk <https://3dr.com/wp-content/uploads/2014/03/pixhawk-manual-rev7.pdf>`_ autopilot with gps module running `ArduCopter <http://ardupilot.org/copter/>`_

- 6x `30A ESCs <https://hobbyking.com/en_us/turnigy-multistar-30a-slim-v2-esc-with-blheli-opto-2-6s.html>`_

- 1x `RFD900 <http://cuairautopilot.readthedocs.io/en/latest/autopilot_configuration.html#id3>`_ -- setup is identical to the plane radios.

ArduCopter setup is very similar to ArduPlane setup and is completely documented `here <http://ardupilot.org/copter/docs/initial-setup.html>`_. The key difference is `ESC calibration <http://ardupilot.org/copter/docs/esc-calibration.html>`_ which must be re-done after major system changes like receiver replacement or motor replacement. 

Atlas flight instructions can be found `here <https://docs.google.com/a/cornell.edu/document/d/1p07ljg8zF4MjSbM2MOx6P8YeXyO3xy7GhyED3VAolZA/edit?usp=sharing>`_.