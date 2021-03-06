========================================================================
UHD - Internal GPSDO Application Notes (USRP-X3x0 Models)
========================================================================

.. contents:: Table of Contents

This application note describes the use of the board-mounted GPS Disciplined OCXO, 
as used with the USRP X300/X310.  For information regarding the GPSDO that is 
compatible with the USRP N2xx or E1xx, please see:

`USRP-N2x0/E1x0 Internal GPSDO Device Manual <./gpsdo.html>`_

------------------------------------------------------------------------
Specifications
------------------------------------------------------------------------
* **Receiver type**: 50 channel with WAAS, EGNOS, MSAS
* **10 MHz ADEV**: 5e-11 over >24h
* **1PPS RMS jitter**: <50ns 1-sigma
* **Holdover**: <20us over 3h

**Phase noise**:

+------------+------------+
|            |    OCXO    |
+============+============+
| **1Hz**    | -75dBc/Hz  |
+------------+------------+
| **10Hz**   | -110dBc/Hz |
+------------+------------+
| **100Hz**  | -132dBc/Hz |
+------------+------------+
| **1kHz**   | -142dBc/Hz |
+------------+------------+
| **10kHz**  | -145dBc/Hz |
+------------+------------+
| **100kHz** | -150dBc/Hz |
+------------+------------+

**Antenna Types:**

The GPSDO is capable of supplying a 3V for active GPS antennas or supporting passive antennas.

------------------------------------------------------------------------
Installation Instructions
------------------------------------------------------------------------
To install the GPSDO, you must insert it into the slot on the board
near the 10 MHz Reference SMA. Keep in mind that the two sides of the
GPSDO have a different number of pins. When inserting the GPSDO, make
sure to press down firmly and evenly. When turning on the USRP B2X0 device,
a green LED should illuminate on the GPSDO. This signifies that the unit
has successfully been placed.

**NOTE: The pins on the GPSDO are very fragile. Be sure to press down
evenly, or the pins may bend or break. Once the GPSDO is in place,
we very highly discourage further removal, as this also risks damaging
the pins.**

------------------------------------------------------------------------
Using the GPSDO in Your Application
------------------------------------------------------------------------
By default, if a GPSDO is detected at startup, the USRP will be configured
to use it as a frequency and time reference. The internal VITA timestamp
will be initialized to the GPS time, and the internal oscillator will be
phase-locked to the 10MHz GPSDO reference. If the GPSDO is not locked to
satellites, the VITA time will not be initialized.

GPS data is obtained through the **mboard_sensors** interface. To retrieve
the current GPS time, use the **gps_time** sensor:

::

    usrp->get_mboard_sensor("gps_time");

The returned value will be the current epoch time, in seconds since
January 1, 1970. This value is readily converted into human-readable
format using the **time.h** library in C, **boost::posix_time** in C++, etc.

Other information can be fetched as well. You can query the lock status
with the **gps_locked** sensor, as well as obtain raw NMEA sentences using
the **gps_gprmc**, and **gps_gpgga** sensors. Location
information can be parsed out of the **gps_gpgga** sensor by using **gpsd** or
another NMEA parser.
