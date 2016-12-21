.. CUAir Distributed Systems Documentation documentation master file, created by
   sphinx-quickstart on Mon May  2 11:28:43 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


Geotag Documentation
============================

.. contents::

This section provides a mathematical explanation for how Distributed Systems handles image geotagging.

Constants
----------------

* :math:`FOV_H` :  the horizontal field of view of the camera lens in radians (`source <https://www.tamron.co.jp/en/data/cctv_fa/m118fm08.html/>`_)

* :math:`FOV_V` :  the vertical field of view of the camera lens in radians (`source <https://www.tamron.co.jp/en/data/cctv_fa/m118fm08.html/>`_)

* :math:`IMAGE \ WIDTH` :  the width of the image captured by the camera in pixels

* :math:`IMAGE \ HEIGHT` :  the height of the image captured by the camera in pixels

Math
-------

1. Figure out how much of the ground is captured by each image (in feet). Since the vertical and horizontal field of view is different calculate distance imaged for both axes. Note that the altitude is measured above ground.

.. image:: images/geotag-math-diagram.png

:math:`Horizontal \ distance \ imaged = 2 \times altitude \times \tan\left(\frac{FOV_H}{2}\right)`

:math:`Vertical \ distance \ imaged = 2 \times altitude \times \tan\left(\frac{FOV_V}{2}\right)`

2. Figure out how much distance is covered by each pixel in the image. Note that since the camera sensor cells are not square, the horizontal and vertical distance covered is different.

:math:`Horizontal \ distance \ covered \ per \ pixel \ = \ Horizontal \ distance \ imaged / IMAGE \ WIDTH`

:math:`Vertical \ distance \ covered \ per \ pixel \ = \ Vertical \ distance \ imaged / IMAGE \ HEIGHT`

3. The pixelX, pixelY location of the target is from the top left of the image. Since the plane rotation is around the center of the image, we want to find the pixelX and pixelY location of the target using that coordinate frame.

:math:`Relative \ pixel \ X = pixelX - \left(\frac{IMAGE \ WIDTH}{2}\right)`

:math:`Relative \ pixel \ Y = \left(\frac{IMAGE \ HEIGHT}{2}\right) - pixelY`

4. Rotate the pixel location based on the plane’s yaw. Basically, the plane is rotated from North by the plane Yaw, and the target’s pixel location in the image is in this rotated reference frame. In order to calculate the latitude and longitude of the target, we want to put the target’s pixel location in a reference frame where North corresponds with the positive Y axis and East corresponds with the positive X axis.

The above corresponds to a rotation of negative plane yaw in ENU. The image coordinate is in ENU, but the plane yaw we get from the autopilot is in NED. As a result, we rotate by negative of negative of plane yaw.

Rotate :math:`\left(relative \ pixel \ x \ , \ relative \ pixel \ , \ 0 \right)` where axis is :math:`(0, 0, 1)` and angle is :math:`planeYaw` to get :math:`rotated \ pixel \ X` and :math:`rotated \ pixel \ Y`

5. Calculate the resulting delta latitude and longitude. Based on the location where we’re flying, we can get feet per degree of latitude and longitude from the Internet.

:math:`\Delta latitude = \left(Rotated \ pixel \ X \times Horizontal \ distance \ covered \ per \ pixel \right) / Feet \ per \ degree \ latitude`

:math:`\Delta longitude = \left(Rotated \ pixel \ Y \times Vertical \ distance \ covered \ per \ pixel \right) / Feet \ per \ degree \ longitude`

6. Now we can just add the delta latitude and longitude to the GPS location of the plane when the image was taken to get the target latitude and longitude.
