.. CUAir Distributed Systems Documentation documentation master file, created by
   sphinx-quickstart on Mon May  2 11:28:43 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


Camera Server
============================

.. contents::

This section provides the use and specifications of the distributed systems camera server.

Overview
----------------

The CUAir camera server is designed to capture images and manage camera settings. Image transferring is handled by the camera server posting the ground server with an image.

**Full documentation of the 2016-2017 Camera Server API can be found** `here <http://docs.cuair20162017cameraserverapi.apiary.io/>`_.

The camera server is built using the NodeJS web framework. The camera that will be used for the 2017 SUAS Competition is the `Z-Cam <http://z-cam.com/>`_.

Design Decisions
------------------------

* Decision to use Z-Cam

  * Were able to capture high quality images at altitude on Atlas
  * Were able to mitigate rolling shutter effects with a simulation on the ground using the Atlas’ vibrations
  * Communicated with electrical to ensure that the OBC could send requests to the Z-Cam over its WiFi while simultaneously running a server on electrical’s WiFi
  * Simplicity of HTTP API far surpasses Point Grey’s poor SDK
  * Reliability of capturing far surpases Point Grey’s

* Decision to use Node.js

  * High performance and inherently concurrent (important for continuously accessing images from the camera over HTTP)
  * Conducive to developing HTTP API’s
  * Code brevity
  * Easy to use (relatively)

* Decision to use Express

  * Easier interface for developing HTTP API’s
  * Code brevity

* Decision to use Sqlite

  * Size of settings and state databases is very small so the simplicity of Sqlite outweighs its lack of efficiency at scale

Field of View
---------------

* :math:`h` : half the height of the lens

* :math:`f` : focal length

Horizontal No Zoom
^^^^^^^^^^^^^^^^^^^^

:math:`2 \times \arctan{\left(\frac{18}{22.75}\right)} = 76.7° \ (approx. \ 80°)`
:math:`2 \times \arctan{\left(\frac{h}{f}\right)} = 2 \times \arctan{\left(\frac{.375”}{12mm}\right)} = 2 \times \arctan{\left(\frac{.375}{0.472441}\right)} = 76.88° \ (approx. \ 80°)`

Horizontal Full Zoom
^^^^^^^^^^^^^^^^^^^^

:math:`2 \times \arctan{\left(\frac{18}{63.3}\right)} = 31.75° \ (approx. \ 30°)`
:math:`2 \times \arctan{\left(\frac{h}{f}\right)} = 2 \times \arctan{\left(\frac{.375”}{32mm}\right)} = 2 \times \arctan{\left(\frac{.375}{1.25984}\right)} = 33.15° \ (approx. \ 30°)`

Vertical No Zoom
^^^^^^^^^^^^^^^^^^^^

:math:`2 \times \arctan{\left(\frac{18}{31.5}\right)} = 59.49° \ (approx. \ 60°)`

Vertical Full Zoom
^^^^^^^^^^^^^^^^^^^^
:math:`2 \times \arctan{\left(\frac{18}{85.5}\right)} = 23.777°`

Installation for Development
----------------------------

There are two options for capturing images and changing settings wirelessly from the Z-Cam. One uses a web front end developed by Distributed Systems while the other directly leverages the HTTP API provided by the Z-Cam via the Postman HTTP client.

**NOTE: please note down all meaningful settings (shutter speed, aperture, iso, zoom etc.) when capturing images for testing purposes.**

Using the Web Front End
^^^^^^^^^^^^^^^^^^^^^^^^^

1. Install `NodeJS <https://nodejs.org/en/download/>`_
2. Install `git <https://git-scm.com/book/en/v2/Getting-Started-Installing-Git/>`_

3. Access the Z-Cam server
::

   git clone https://github.com/CUAir/z-cam-server.git
   cd z-cam-server/
   git checkout capturing
   npm install


4. Turn on the Z-Cam
5. Remove lens (but do not lose it!)
6. Set the camera zoom to full zoom or minimum zoom (but record which one you are using for testing purposes!)
7. Take one picture manually pointing at a very far distances (this focuses the camera)
8. Navigate to the settings and check that WiFi is on
9. Connect to Z-Cam’s WiFi network on your computer (password: 12345678)

10. Start the web server
::

   npm start

11. Use the web front end to capture images and change settings
12. If you use the software at a test flight, please contact Ram to export the image and settings information from your computer so Distributed can save the data for testing purposes
13. Please submit any issues with the front end to our `Github repository <https://github.com/CUAir/z-cam-server/>`_ and/or contact Ram

Direct Requests Using Postman
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Install `NodeJS <https://nodejs.org/en/download/>`_
2. Install `git <https://git-scm.com/book/en/v2/Getting-Started-Installing-Git/>`_
3. Install `Postman <https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=en/>`_
4. Turn on the Z-Cam
5. Remove lens (but do not lose it!)
6. Set the camera zoom to full zoom or minimum zoom (but record which one you are using for testing purposes!)
7. Take one picture manually pointing at a very far distances (this focuses the camera)
8. Navigate to the settings and check that WiFi is on
9. Connect to Z-Cam’s WiFi network on your computer (password: 12345678)
10. To change Z-Cam settings, use Postman to send a ``GET`` request to ``http://10.98.32.1:80/ctrl/set?{setting}={value}``. Possible settings and values are outlined in the "Settings" section of this document.
11. To capture a single image on the Z-Cam, use Postman to send a ``GET`` request to ``http://10.98.32.1:80/ctrl/still?action=single``.
12. If you use the software at a test flight, please contact Ram to export the image and settings information from your computer so Distributed can save the data for testing purposes
13. Please submit any issues with the front end to our `Github repository <https://github.com/CUAir/z-cam-server/>`_ and/or contact Ram

Z-Cam Server API Overview
--------------------------

Properties
^^^^^^^^^^

* Battery (integer, 0, 100)

Settings
^^^^^^^^

* Brightness (integer, -256, 256)
* Saturation (integer, 0, 256)
* Sharpness ("Weak","NormalNoise","Strong")
* Contrast (integer, 0, 256)
* Exposure Value (integer, -96, 96)
* Meter mode (“Center”, “Average”, “Spot”)
* Flicker (“Auto”, “60Hz”, “50Hz”)
* ISO ("Auto", "100", "125", "160", "200", "250", "320", "400", "500", "640", "800", "1000", "1250", "1600", "2000", "2500", "3200", "4000", "5000", "6400”)
* White Balance (“Auto”, “Manual”)
* Aperture ("5.6", "6.3", "7.1", "8", "9", "10", "11", "13", "14", "16", "18", "20", "22”)
* Auto-Focus Mode (“Normal”, “Selection”)
* Focus (“MF”, “AF”)
* Continuous Auto-Focus (“0”, “1”)
* Burst (“Off”, “On”)
* Drive Mode (“single”, “continuous”, “time_lapse”)

Capturing
^^^^^^^^^
* Capture Image
