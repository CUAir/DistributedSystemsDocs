.. CUAir Autopilot Documentation documentation master file, created by
   sphinx-quickstart on Mon May  2 11:28:43 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Autopilot documentation
=========================================================

Welcome
=========

Yay, documentation!


How to edit this page
---------------------

Easy Mode
^^^^^^^^^

- Click the "Edit on GitHub button at the top of any page"
- Edit the page
- Click 'Preview changes' before committing to make sure you haven't made a mistake with markdown syntax.
- Add a change message and commit when you have finished your changes
- In a few minutes, readthedocs will be updated

Leet Mode
^^^^^^^^^

**Installation**

::

   $ git clone https://github.com/CUAir/Autopilot
   $ cd Autopilot
   $ virtualenv venv 
   $ source venv/bin/activate
   $ pip install -r requirements.txt
   $ cd docs
   $ make html

**Use** 

* To create a page, make a new file called foobar.rst. 
* Add your page to the table of contents in the file index.rst under the api and groundstation pages. 
* Edit your page using sphinx markup. 
* To see what your page looks like, compile it with ``make html``
* When you are happy with what the page looks like commit your changes with git (``git add --all; git commit -m "change message"; git push``).
* The online documentation will update sometime in the next few minutes.


To learn more about how to use sphinx, see the following guides

http://www.sphinx-doc.org/en/stable/tutorial.html

http://www.sphinx-doc.org/en/stable/rest.html#rst-primer

Support
-------

If you are having issues, please slack Troy.

License
-------
The project may or may not have a license.


Contents:

.. toctree::
   :maxdepth: 2

   api
   groundstation
   ardupilot
   catapult
   testflight
   autopilot_configuration

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

