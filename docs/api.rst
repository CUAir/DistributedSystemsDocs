.. CUAir Autopilot Documentation documentation master file, created by
   sphinx-quickstart on Mon May  2 11:28:43 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


Autopilot API endpoints
============================

.. contents::

The API for communicating between the autopilot and the ground station.

Status [/status]
----------------

Get the status of the plane: A large json containing each piece of data as a name/value pair. A call can also be made to /status/... to receive an
individual bit of data (below)::

  Response 200 (application/json)
        {
            "attitude" : {
                "pitch" : 0.0,
                "yaw" : 0.0,
                "roll" : 0.0,
                "pitchspeed" : 0.0,
                "yawspeed" : 0.0,
                "rollspeed" : 0.0
                },
            "battery" : {
                "pct" : 0.0,
                "voltage" : 0.0
                },
                
            "link" : {
                "gps_link" : "True",
                "plane_link" : "True"
                },
            
            "time" : 0,
            
            "gps" : {
                "rel_alt" : 0.0,
                "asl_alt" : 0.0,
                "lat" : 0.0,
                "lon" : 0.0,
                "heading" : 0.0,
                "vx" : 0.0,
                "vy" : 0.0,
                "vz" : 0.0
            },
            "mode": "AUTO",
            "airspeed" : {
                "vx" : 0.0,
                "vy" : 0.0,
                "vz" : 0.0
            },
            
            "wind" : {
                "vx" : 0.0,
                "vy" : 0.0,
                "vz" : 0.0
            },
            
            "signal" : {
                "time" : 0.0,
                "signal_strength": 0
            },
            
            "flight_time" : {
                "time" : 0.0,
                "time_start" : 0.0,
                "is_flying" : "False"
            },
            
            "gps_status" : {
                "time" : 0.0,
                "satellite_number": 0
            },
            
            "throttle" : 0,
            
            "waypoints" : [{
                "alt" : 0.0,
                "lon" : 0.0,
                "lat" : 0.0
            }],
            
            "wp_count" : 0,
            "current_wp" : 0,
            "geofences" :  [{
                "lat" : 0.0,
                "lon" : 0.0
            }],
            "hud" : {
            "airspeed" : 0.0,
            "groudspeed": 0.0,
            "heading": 0,
            "throttle": 0,
            "alt": 0.0,
            "climb": 0.0
            }
        }

Attitude [/status/attitude]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Returns the plane's attitude, containing:

* Pitch [float]
* Yaw [float]
* Roll [float]
* Pitchspeed [float]
* Yawspeed [float]
* Rollspeed [float]

::

  + Response 200 (application/json)
  { 
     "pitch" : 0.0,
     "yaw" : 0.0,
     "roll" : 0.0,
     "pitchspeed" : 0.0,
     "yawspeed" : 0.0,
     "rollspeed" : 0.0,
   }

Battery [/status/battery]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns the current state of the plane's battery, containing:

* pct [float]
* voltage [float]

::

 + Response 200 (application/json)
        {
            "pct" : 0.0,
            "voltage" : 0.0,
        }
        
Link [/status/link]
^^^^^^^^^^^^^^^^^^^

Returns the status of links, containing:

* gps_link [boolean]
* plane_link [boolean]

::

 + Response 200 (application/json)
        {
            "gps_link" : "True",
            "plane_link" : "True",
        }
        
Time [/status/time]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns the current time as an long representing a [unix timestamp](https://en.wikipedia.org/wiki/Unix_time) 


::

  + Response 200 (application/json)
        {
           0
        }
        
GPS [/status/gps]
^^^^^^^^^^^^^^^^^^^^^^^^

Returns various values from the plane's onboard GPS, containing:

* rel_alt [float]
* asl_alt [float]
* lat [float]
* lon [float]
* heading [float]
* vx [float]
* vy [float]
* vz [float]

::

  + Response 200 (application/json)
        {
            "rel_alt" : 0.0,
            "asl_alt" : 0.0,
            "lat" : 0.0,
            "lon" : 0.0,
            "heading" : 0.0,
            "vx" : 0.0,
            "vy" : 0.0,
            "vz" : 0.0,
        }
        
Mode [/status/mode]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns the current flying mode of the plane as a string, e.g. "AUTO", "MANUAL", "FLY_BY_WIRE_A"

::

 Response 200 (application/json)
        {
           "AUTO"
        }
        
Airspeed [/status/airspeed]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns vectors vx, vy, vz representing the airspeed velocity of the airplane as floats

::

 + Response 200 (application/json)
        {
            "vx" : 0.0,
            "vy" : 0.0,
            "vz" : 0.0
        }

Wind [/status/wind]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns vectors vx, vy, vz representing the wind velocity vector as floats

::

 Response 200 (application/json)
        {
            "vx" : 0.0,
            "vy" : 0.0,
            "vz" : 0.0
        }    

Signal [/status/signal]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns the time and the signal strength as an integer of the radio connection

::

 + Response 200 (application/json)
        {
            "time" : 0.0,
            "signal_strength": 0
        }
        
Flight Time [/status/flight_time]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns the information about the flight time conntaing:

* time_start [float]
* if_flying [boolean]

::

 + Response 200 (application/json)
        {
            "time" : 0.0,
            "time_start" : 0.0,
            "is_flying" : "False"
        }
        
GPS Status [/status/gps_status]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns the gps connection represented by an integer number of satellites visable

::

 + Response 200 (application/json)
        {
            "time" : 0.0,
            "satellite_number": 0
        }

Throttle [/status/throttle]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

An integer from 0 to 100 representing the current throttle level of the plane

::

 Response 200 (application/json)
        {
            0
        }
        
Waypoints [/status/waypoints]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns a list of JSON objects representing the current waypoints altitude, latitude, and longitude

::

 + Response 200 (application/json)
        [{
                "alt" : 0.0,
                "lon" : 0.0,
                "lat" : 0.0,
        }]
        
Waypoint Count [/status/wp_count]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns an integer representing the current number of waypoints

::

 + Response 200 (application/json)
        {
            0
        }
        
Current Waypoint [/status/current_wp]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns an integer representing the current waypoint

::

 + Response 200 (application/json)

        {
            0
        }
        
Geofence [/status/geofences]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns a list of JSON objects representing the latitude and longitude of the geofences

:: 

 Response 200 (application/json)
        [{
            "lat" : 0.0,
            "lon" : 0.0,
        }]

HUD [/status/hud]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns a list of values needed for the HUD, containing,

* airspeed [float]
* groundspeed [float]
* heading [integer]
* throttle [integer]
* alt [float]
* climb [float]

:: 

 Response 200 (application/json)
        {
            "airspeed" : 0.0,
            "groudspeed": 0.0,
            "heading": 0,
            "throttle": 0,
            "alt": 0.0,
            "climb": 0.0
        }

Software Status [/status/softstatus?time=TIME]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Use the GET argument "time" (/status/softstatus?time=TIME) to request a status at a specific time. If an exact value is not available, an interpolated value will be provided.

::

 Response 200 (application/json)
        {      
        attitude: {
            'roll': 0,
            'pitch': 0,
            'yaw': 0,
            'rollspeed': 0,
            'yawspeed': 0,
            'pitchspeed': 0
            
        },
        gps:{
             lat: 0,
             lon: 0,
             asl_alt: 0,
             vx: 0,
             vy: 0,
             vz: 0,
             heading: 0,
             rel_alt: 0
         },
         airspeed:{
             'vx': 0,
             'vy': 0,
             'vz': 0
         },
         wind: {
             'vx': 0,
             'vy': 0,
             'vz': 0
         }


Waypoints [/wp]
-----------------

* **GET**

Returns a list of waypoints, each containing, altitude, longitude, latitude, current waypoint, waypoint type or `MAV_CMD <http://mavlink.org/messages/common>`_ , waypoint index::

 Response 200 (application/json)
        [{
            "alt" : 0.0, [meters]
            "lon" : 0.0, [degrees]
            "lat" : 0.0, [degrees]
            "current": 0, 
            "type": 12, 
            "index": 0 
        }, 
        {
            "alt" : 0.0,
            "lon" : 0.0,
            "lat" : 0.0,
            "current": 0,
            "type": 16,
            "index": 0
        }]
    
*  **GET with arguments [GET /wp/{?wpnum}]**

The response field, "type" in GET is the same as the "command" field in POST and PUT. 
The associated waypoint types and numbers are listed under POST. 

Parameters: *wpnum*  - the index of the waypoint you wish to recieve::

  Response 200 (application/json)

        {
            "alt" : 0.0,
            "lon" : 0.0,
            "lat" : 0.0,
            "current": 0,
            "type": 21,
            "index": 0
        }
        
* **DELETE**
   Delete a specific waypoint.
   
   Parameters: *wpnum*  - The waypoints index

::

   Response 200 (application/json)
        "True"

* **POST**


::

   Headers
      Content-Type: application/json
      token: <secret token>

   Requests
      "lat: <lat>,        [The waypoint's latitude]
      lon: <lon>,        [The waypoint's longitude]
      alt: <alt>,        [The waypoint's altitude]
      index: <index>,    [The waypoints index]
      commant: <command> [The waypoints type or `MAV_CMD <http://mavlink.org/messages/common>`]

   Response 200 (application/json)
        "True"

* **PUT**

   PUT has the same parameters as POST but will update the values of the waypoint at the specified index.

::

   Headers
      Content-Type: application/json
      token: <secret token>

   Requests:
    "lat: <lat>,        [The waypoint's latitude]
     lon: <lon>,        [The waypoint's longitude]
     alt: <alt>,        [The waypoint's altitude]
     index: <index>,    [The waypoints index]
     commant: <command> [The waypoints type or `MAV_CMD <http://mavlink.org/messages/common>`]

   Response 200 (application/json)
        "True"


Interop [/interop]
------------------


Server Control [ground/api/v3/interop]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
* **POST**

  Sending a POST request to this endpoint starts the interop backend. To do this, it creates a new instance of the backend object, then starts the backend on a separate thread and sets the server to active. It will fail if the server is either already started, or if it has been less that a half second since the server was either started or stopped last. Requires a valid JSON containing the server data (username, password, and url fields). Requires a valid auth token to access. ::

    Response 200


* **DELETE**

  Sending a DELETE request to this endpoint will stop the interop backend. It simply sets the Data.server_active global variable to false. This is the loop condition on the backend, so the server will stop as soon as it completes its current loop. This will fail if the server is either already stopped or if it has been less that a half second since the server was either started or stopped last. Requires a valid auth token to access ::

    Response 200


* **GET**

  Returns a JSON string containing all available server info

  * "Obstacles" : Data structure containg obstacles ({"moving_obstacles":[],"stationary_obstacles":[]})
  * "server_working" : Does the server believe it is functioning correctly (boolean)
  * "hz" : Rolling frequency of interop telemetry posts (integer)
  * "active" : Is the server active (boolean)
  * "wp_distances" : Closest point of approach to each waypoint (integer list)
  * "active_mission" : JSON of active mission as described by the `interop documentation <http://auvsi-suas-competition-interoperability-system.readthedocs.io/en/latest/specification.html#missions>`_. 

  ::

    Response 200 (application/json)
    {  
        "hz":2.7496117782366105,
        "obstacles":{  
            "moving_obstacles":[  
                {  
                    "latitude":38.143752406998416,
                    "sphere_radius":15.239999976835199,
                    "altitude_msl":38.77596404856716,
                    "longitude":-76.4332677324261,
                    "time":1480738099.504048
                }
            ],
            "stationary_obstacles":[  
                {  
                    "latitude":38.14792,
                    "cylinder_height":60.959999907340794,
                    "cylinder_radius":45.7199999305056,
                    "longitude":-76.427995
                },
                {  
                    "latitude":38.145823,
                    "cylinder_height":91.4399998610112,
                    "cylinder_radius":15.239999976835199,
                    "longitude":-76.422396
                }
            ]
        },
        "wp_distances":[  
            0.07176477460652146,
            52572731.79846973,
            52572653.50093492,
            52572646.28086038,
            52572701.55982889
        ],
        "active_mission":{  
            "fly_zones":[  
                {  
                    "boundary_pts":[  
                        {  
                            "latitude":38.142544,
                            "order":1,
                            "longitude":-76.434088
                        },
                        {  
                            "latitude":0.0,
                            "order":1,
                            "longitude":0.0
                        },
                        {  
                            "latitude":38.141833,
                            "order":2,
                            "longitude":-76.425263
                        },
                        {  
                            "latitude":38.144678,
                            "order":3,
                            "longitude":-76.427995
                        }
                    ],
                    "altitude_msl_max":1000.0,
                    "altitude_msl_min":0.0
                }
            ],
            "off_axis_target_pos":{  
                "latitude":42.4471955938344,
                "longitude":-76.6138759083697
            },
            "mission_waypoints":[  
                {  
                    "latitude":42.4462099439294,
                    "altitude_msl":2179.69165478027,
                    "order":4,
                    "longitude":-76.6105735301971
                },
                {  
                    "latitude":42.4462811962498,
                    "altitude_msl":2179.69165478027,
                    "order":5,
                    "longitude":-76.610374962911
                },
                {  
                    "latitude":-35.3632621765137,
                    "altitude_msl":1917.22445478027,
                    "order":1,
                    "longitude":149.165237426758
                },
                {  
                    "latitude":42.4474133055778,
                    "altitude_msl":2114.07485478027,
                    "order":2,
                    "longitude":-76.610369682312
                },
                {  
                    "latitude":42.4474014304113,
                    "altitude_msl":2179.69165478027,
                    "order":3,
                    "longitude":-76.6106593608856
                }
            ],
            "search_grid_points":[  
                {  
                    "latitude":38.142544,
                    "altitude_msl":200.0,
                    "order":1,
                    "longitude":-76.434088
                }
            ],
            "sric_pos":{  
                "latitude":38.141833,
                "longitude":-76.425263
            },
            "active":true,
            "id":1,
            "home_pos":{  
                "latitude":38.14792,
                "longitude":-76.427995
            },
            "air_drop_pos":{  
                "latitude":38.141833,
                "longitude":-76.425263
            }
        },
        "server_working":true,
        "active":true
    }
    

Obstacles [/ground/api/v3/interop/obstacles]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns a JSON object string that contains a list of both moving and stationary objects. Checks to see if the server is active, and, if so, retrieves data from the MAVProxy.modules.server.data module, jsonifies it and returns it. ::

  Response 200 (application/json)
          {
            stationary_obstacles : [{
                  cylinder_height : 0.0, 
                  cylinder_radius : 0.0, 
                  latitude : 0.0, 
                  longitude : 0.0
                }],
            moving_obstacles : [{
                  altitude_msl : 0.0, 
                  latitude : 0.0, 
                  longitude : 0.0, 
                  sphere_radius : 0.0
            }],
          }

Hz [/ground/api/v3/interop/hz]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns a string containing the rolling average of the frequency that the interop server has been posting telemetry data ::

  Response 200
          10.15234

Server Active [/ground/api/v3/interop/active]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns a boolean string telling whether the interop server is currently active or not ::

  Response 200
          true

Waypoints [/ground/api/v3/interop/wp]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns an integer list giving the closest point of approach to each waypoint ::

  Response 200
          [  
            0.3071459946680728,
            854.5473948275072,
            1768.1771508733752,
            1394.3356031300505
          ]

Status [/ground/api/v3/interop/status]
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Returns a boolean string telling whether the interop server believes it is working as intended right now. Automatically true if the server is not active ::

  Response 200
          true
