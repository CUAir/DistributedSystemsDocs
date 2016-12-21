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
