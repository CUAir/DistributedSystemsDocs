Autopilot Configuration and Setup
=================================

.. contents::

Overview
---------

This page documents remaining autopilot configuration procedures that do not relate to ardupilot software.

Setting up RFD900s
-------------------

We use `RFD900+ <http://store.rfdesign.com.au/rfd-900p-modem/>`_ radio modems. Please read the `data sheet <http://files.rfdesign.com.au/Files/documents/RFD900%20DataSheet.pdf>`_ before attempting to change the modem settings.

1. Firmware can be installed on RFD900s with windows using the RFD900 configuration tool. This can be found `here <http://files.rfdesign.com.au/docs/>`_.

2. Settings should match below guide. Make sure to use the same netid on both nodes of a piar and use different netids on different pairs. If you have different settings than shown, you may need update your firmware  ::

    Baudrate 115200
    Airspeed 250
    NetID  xx
    Tx Power 27
    NodeID 1             # GROUND/BASE NODE SHOULD HAVE THESE REVERSED
    Node Destination 0   # GROUND/BASE NODE SHOULD HAVE THESE REVERSED

    Min Freq 902000
    Max Freq 928000
    Number of Channels 50
    Duty Cycle 100
    Node Count 2

    ECC (unchecked)
    Mavlink (checked)
    Sync any (unchecked)
    Op Resent (checked)
    RTCSTS (unchecked)



3. You can check that two RFD900s are paired and communicating with screen. In terminal, run the command ::

    screen /dev/tty.usb<DEVICE_NAME> <BAUDRATE>

For example  ::

    screen /deb/tty.usbmodem1 115200


Binding Transmitter and Receiver
--------------------------------

We use an `EzUHF 4 Channel Receiver <http://www.immersionrc.com/fpv-products/ezuhf-4ch-receiver/>`_ and an `EzUHF Transmitter JR Module <http://www.immersionrc.com/fpv-products/ezuhf-jr-module/>`_ on a Taranis Transmitter. Refer to their respective documentation if there is any confusion about the following binding procedure.


1. Download `ImmersionRC update config tool software <http://www.immersionrc.com/?download=4894>`_

2. Connect the transmitter to the computer with the config software.

3. On the software, set the transmitter to extreme hopping

4. Connect the receiver to the computer with the config software

5. On the receiver, set the frequency band to match the transmitter frequency band

6. On the servo mapping tab set PPM slot and servo output as follows ::

    PPM1 CH2
    PPM2 CH3
    PPM3 CH1
    PPM4 CH4
    PPM5 CH5
    PPM6 CH6
    PPM7 CH7
    PPM8 CH8

    CH1 PPM Muxed
    CH2 -
    CH3 -
    CH4 -

7. Put the transmitter into bind mode by turning it off and then holding the bind button on the module on the back as it turns on. It should start beeping slowly.

8. Put the receiver into bind mode with the bind tab in the software. 

9. The software should tell you that you have successfully bound the receiver!


Wiring guide
-------------

+------------+------+----------------+----------------------+
| Output pin | Mode | Surface        | Inverted Transmitter |
+------------+------+----------------+----------------------+
| 8          | 19   | Left Elevator  | Yes                  |
+------------+------+----------------+----------------------+
| 9          | 21   | Rudder         |                      |
+------------+------+----------------+----------------------+
| 11         | 2    | Left Flap      |                      |
+------------+------+----------------+----------------------+
| 5          | 4    | Left Aileron   | Yes                  |
+------------+------+----------------+----------------------+
| 10         | 2    | Right Flap     |                      |
+------------+------+----------------+----------------------+
| 12         | 4    | Right Aileron  | Yes                  |
+------------+------+----------------+----------------------+
| 3          | na   | Throttle       |                      |
+------------+------+----------------+----------------------+
| 7          | 19   | Right Elevator | Yes                  |
+------------+------+----------------+----------------------+
