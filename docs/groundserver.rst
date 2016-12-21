.. CUAir Distributed Systems Documentation documentation master file, created by
   sphinx-quickstart on Mon May  2 11:28:43 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


Ground Server
============================

.. contents::

This section provides the use and design of the distributed systems ground server.

Overview
----------------

The ground server is designed to fulfill two tasks: target detection/localization and delivering a care package (airdrop). In order to fulfill these tasks, the ground server must keep track of and store various settings and states. More importantly, it should be able to handle client requests reliably. Full documentation of the 2016-2017 Ground Server API can be found `here <http://docs.cuair20162017groundserverapi.apiary.io/>`_.

The CUAir ground server is built using the Play web framework in Java. It’s an MVC framework that separates the logic for the view (our frontend), controller (API endpoints that allow clients/servers to communicate with us), and model (interfacing with the database layer, running any algorithms or business logic).


Design
-------

Models
^^^^^^^

Below is a class diagram of the ground server models. One can see the one-to-one as well as many-to-one relationships (more in next section).

.. image:: images/ground-server-diagram.png

Many-to-One Relationship

.. image:: images/many-to-one-diagram.png

The above figure demonstrates the “one-to-many” relationships between the ground server abstractions. Each image has multiple assignments, which are distributed among various clients, and each assignment can have multiple target sightings.

While this accurately represents the relationship among our abstractions, our software design takes a different approach:

* Target Sighting

  * Assignment

    * Image

* Target Sighting

  * Assignment

    * Image

* Target Sighting

  * Assignment

    * Image

In this approach, we see that there is a “many-to-one” relationship between TargetSighting and Assignment, and between Assignment and Image. The reason we take this approach rather than “one-to-many” is that for one, many-to-one is much simpler and cleaner to serialize into an SQL database. Additionally, this design accurately represents the underlying operations of the ground server. Whether or not TargetSightings are added into an Assignment, Assignment is only concerned with the Image to which it was assigned. Similarly, Image should not bother with Assignments, as it is only concerned with the image data itself.

The ground server models are used to store data in a SQL database through serialization. The ground server utilizes `Ebean <http://ebean-orm.github.io/>`_ to handle this serialization. Ebean is an Object Relational Mapping (ORM), which is a Java library that allows us to execute SQL commands on our database tables.

Database Accessor Objects
^^^^^^^^^^^^^^^^^^^^^^^^^

For each model in the ground server, there exists a database accessor object, or DAO. DAOs utilize Ebean methods to retrieve data from the SQL database. DAOs are an abstraction around accessing the the database from the controller, as many of the methods used to retrieve data are similar across the controllers (get, create, delete, update). The DAO combines these methods into one interface that allows us to handle these requests for any CUAirModel. When we want to make more complicated requests, we can simply extend the DAO and add the necessary method. (i.e. retrieving all target sightings for a certain image). The DAO abstraction is also useful as it prevents us from accessing the database directly. So, if we need to migrate to another ORM or library, we will simply need to modify the DAO rather than the controller code, which would be more complex.

Clients
^^^^^^^

The client abstractions are designed to process requests to get and set settings and state of the plane servers (Gimbal, Camera, Airdrop, Autopilot). Due to the possibility of a failed connection, the client abstractions include threads separate to the application thread that are meant to continue trying to send requests up to the server until a non-timeout response is received.

The underlying pattern with the Client abstractions is that each server on the plane (Gimbal, Airdrop, Camera) contains a client class which handles requests to set the settings, as well as to get the settings and/or state.

ImageClient is a unique case which involves obtaining information from Autopilot and the Gimbal in order to get the telemetry data for a particular image. Since all of the plane servers are on the same onboard computer, they have the same timestamp. This plane timestamp, therefore, can be taken from the Image and used in the queries in AutopilotClient and GimbalClient. ImageClient runs two parallel threads which attempt to get autopilot telemetry data and the gimbal state for an image, respectively.

Client Class Diagram
********************

.. image:: images/client-class-diagram.png

The Client abstraction simply defines a thread that continuously executes run().

The SettingsClient abstraction contains a queue of requests and extends Client. The setSettings() method, which is called by the Client, will add the request to the queue and return a 200 response as an indication that the request was successfully received and is currently being processed. When it is run, it will poll the queue and attempt to send the request (if any) to the server. Once a 200 response is received in the thread, indicating that the settings were successfully sent to the server, the update gets reflected on the front-end. This is extended by CameraClient.

StateSettingsClient, which extends SetttingsClient, allows one to get state. This is extended by AirdropClient and GimbalClient.

AutopilotClient simply gets autopilot telemetry data at a particular timestamp and has no concept of changing the settings or state. Therefore, it is not extended by any client abstractions.

ImageClient is a unique case which involves obtaining information from Autopilot and the Gimbal in order to get the telemetry data for a particular image. Since all of the servers are on the same computer, they have the same timestamp. This timestamp, therefore, can be taken from the Image and queried for in AutopilotClient and GimbalClient. ImageClient runs two parallel threads which attempt to get autopilot telemetry data and the gimbal state, respectively.

Settings and States
******************

The "state" is information that the plane inherently knows that the ground server cannot directly change but can certainly query for. The plane settings, however, are directives of the plane and can be changed by the ground server. A change in setting can and does induce a change in state. The state and the settings breakdown for the plane servers as follows:

* **Airdrop Server**

  * State: Whether the drop has occured or not (the ground server can try to arm/override but only the plane knows whether the physical mechanism was activated)

  * Settings: Target latitude and longitude, acceptable threshold for drop accuracy, arm and disarm, override drop

* **Gimbal Server**

  * State: The quaternion values that the gimbal has assumed

  * Settings:  Gimbal mode (retract, ground, gps, angle) and the subsequent values

* **Camera Server**

  * State: None (Ground server can directly change all values pertaining to the camera, therefore they are all settings)

  * Settings: Everything else (`see the Camera Server section to learn more <http://distributed-systems.readthedocs.io/en/latest/cameraserver.html/>`_)


Controllers
^^^^^^^^^^

The controller abstractions are meant to interact directly with Java’s Play framework. (`More information on Play specifications can be found here <https://www.playframework.com/documentation/2.5.x/Home/>`_). They utilize the client and dao methods in order to process client requests and return a meaningful response.

Installation for Development
----------------------------

1. Install `Java 8 <http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html/>`_
2. Install `git <https://git-scm.com/book/en/v2/Getting-Started-Installing-Git/>`_
3. Install `VirtualBox <http://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html/>`_
4. Install `Vagrant <https://www.vagrantup.com/downloads.html/>`_
5. Access ground server through vagrant ::

   git clone https://github.com/CUAir/ground-server.git
   cd ground-server/
   vagrant up
   vagrant ssh                # Now you're on the VM!
   cd ground-server/

6. Start the ground server on port 9000 ::

   ./activator run

To start tests, run ::

   rm -rf conf/evolutions/*
   ./activator clean
   ./activator compile
   ./activator test

To access the database on VM, run ::

   sudo -i -u postgres
   psql -U postgres plaedalus
   exit


Front-End Overview
-------

The ground server front-end is built primarily in `React <https://facebook.github.io/react/docs/getting-started.html>`_ and it’s in ``ground-server/app/assets/javascripts``. However, some parts, specifically those that interact with the backend use `Nuclear <https://optimizely.github.io/nuclear-js/>`_ and most of the stylesheets are written in `LESS <http://lesscss.org/>`_.

Pages
^^^^^

**Location:** ``ground-server/app/assets/javascripts/pages``

These are the individual pages of the frontend that you will see and access. They’re made of the components described in the following section.

* **App**: the default page and is located in ``/javascripts`` rather than in ``/javascripts/pages``. If you want to add any components that are applied to all pages, put it there.

  * Components: Drawer, Header

* **Tag**: the first page that you will encounter when starting the server. Meant primarily for tagging targets from images that are fed from the plane. As of now, it also includes starting and stopping the plane’s mission status.

  * Components: MissionControl, ImageViewer, ColorSelect, ShapeSelect, TypeSelect

* **Merging**: for merging target sightings with targets and creating new targets. All targets are shown and can be deleted.

  * Components: ColorSelect, ShapeSelect, TypeSelect

* **CameraSettings**: controls the camera’s settings and shows what the resulting images look like.

  * Components: ImageViewer

* **GimbalAirdrop**: controls the gimbal and airdrop functions.

  * Components: Airdrop, Gimbal


Components
^^^^^^^^^^
**Location:** ``ground-server/app/assets/javascripts/components``

The individual UI elements of the system that are built as React classes.

* **ColorSelect**: drop down menu to select the color of the target and also assigns a unique id for the selected color in the following format: ``color_select_<integer between 0 and 100,000>_<integer between 0 and 100,000>``

  * Used in: Merge, Tag

* **Drawer**: manages everything in the page below the header. Everything that renders on the page besides the header is wrapped inside of the class “main” which is part of the component. Also sets the sidebar on or off.

  * Used in: all pages (it’s in App)

* **Header**: the top bar of the page and includes a button to give access the sidebar.

  * Used in: all pages (it’s in App)

* **ImageViewer**: the primary way images from the plane are viewed. Also includes the target selector tool (the big circle that is drawn around a target) for manual detection classification and localization (only active in Tag).

  * Used in: CameraSettings, Tag

* **MissionControl**: displays and sets the plane’s mission status through AJAX calls with the API. Note: due to the way the API works, setting the mission status to COMPLETED will prevent any further changes to the mission status. Also, whoever works on this next should use Nuclear instead of AJAX if they can figure out Nuclear.

  * Used in: Tag

* **ShapeSelect**: drop down menu to select the shape of the target and also assigns a unique id for the selected shape in the following format: ``shape_select_<integer between 0 and 100,000>_<integer between 0 and 100,000>``

  * Used in: Merge, Tag

* **Sidebar**: main navigation tool within ground server. Opening and closing is controlled by Drawer.

  * Used in: all pages (it’s in App)

* **TypeSelect**: drop down menu to select the type (alphanum or emergent) of the target and also assigns a unique id for the selected type in the following format: ``type_select_< integer between 0 and 100,000>_<some between 0 and 100,000>``

  * Used in: Merge, Tag

The following two components are in ``ground-server/app/assets/javascripts/pages/gimbalAirdrop``:

* **Airdrop**: controls the airdrop’s settings and allows you to arm and set the airdrop

  * Used in: GimbalAirdrop

* **Gimbal**: controls the gimbal’s settings

  * Used in: GimbalAirdrop

Adding a Component
^^^^^^^^^^^^^^^^^^

Once you create a component, go to ``ground-server/app/org/cuair/ground/views/main.scala.html``. The ``main.scala.html`` file is where all the system’s CSS and Javascript files are linked to.

In a new line in the file, type the following::

  <script type='text/javascript' src='@routes.Assets.versioned("javascripts/components/<component’s name>.js")'></script>

This should allow any page in the ground server to access the new component.

Nuclear
^^^^^^^

**Location:** ``ground-server/app/assets/javascripts/nuclear``

All files built using Nuclear that are meant to allow the frontend to access the databases through API calls using the internal API.

*Actions*: manages functions related to target sightings and targets. Includes API calls for saving, deleting, and updating targets.

Troubleshooting
----------------

Ground Server Cannot Connect to Plane Servers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Make sure laptop can ping NUC
* Make sure plane servers are running
* Make sure plane you’ve updated the /ground-server/conf/application.conf file with NUC IP address and plane server port number
* Make sure you’ve correctly identified plane server port number
* ``fping -ag 10.148.0.0/24`` (List all IP on the local network)

Ground Server Laptop Cannot Ping NUC
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
* Make sure laptop is connected to switch
* Make sure switch is connected to antenna tracker router or directly to NUC
* Make sure you’ve correctly identified NUC IP address
* Make sure the NUC is turned on

Cannot Connect to MDLC UI From My Laptop
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Make sure laptop is connected to switch
* Make sure ground server laptop is connected to switch
* Make sure ground server is running
