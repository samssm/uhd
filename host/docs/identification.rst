=================================
UHD - Device Identification Notes
=================================

.. contents:: Table of Contents

------------------------
Identifying USRP Devices
------------------------
Devices are addressed through key/value string pairs.
These string pairs can be used to narrow down the search for a specific device or group of devices.
Most UHD utility applications and examples have an **--args** parameter that takes a device address, which is expressed as a delimited string.

See the documentation in **types/device_addr.hpp** for reference.

^^^^^^^^^^^^^^^^^^^^^^^^^
Common device identifiers
^^^^^^^^^^^^^^^^^^^^^^^^^
Every device has several ways of identifying it on the host system:

+------------+----------+-----------------------------------------------------------+-------------------------------
| Identifier | Key      | Notes                                                     | Example
+============+==========+===========================================================+===============================
| Serial     | serial   | globally unique identifier                                | 12345678
+------------+----------+-----------------------------------------------------------+----------------------------
| Address    | addr     | unique identifier on a network                            | 192.168.10.2
+------------+----------+-----------------------------------------------------------+-------------------------------
| Resource   | resource | unique identifier for USRP RIO devices (over PCI Express) | RIO0
+------------+----------+-----------------------------------------------------------+-------------------------------
| Name       | name     | optional user-set identifier                              | my_usrp1 (User-defined value)
+------------+----------+-----------------------------------------------------------+----------------------------
| Type       | type     | hardware series identifier                                | usrp1, usrp2, 
+------------+----------+-----------------------------------------------------------+----------------------------

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Device discovery via command line
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Devices attached to your system can be discovered using the **uhd_find_devices** program.
This program scans your system for supported devices and prints
out an enumerated list of discovered devices and their addresses.
The list of discovered devices can be narrowed down by specifying device address args.

::

    uhd_find_devices

Device address arguments can be supplied to narrow the scope of the search.

::

    uhd_find_devices --args="type=usrp1"

    -- OR --

    uhd_find_devices --args="serial=12345678"

^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Device discovery through the API
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The **device::find()** API call searches for devices and returns a list of discovered devices.

::

    uhd::device_addr_t hint; //an empty hint discovers all devices
    uhd::device_addrs_t dev_addrs = uhd::device::find(hint);

The **hint** argument can be populated to narrow the scope of the search.

::

    uhd::device_addr_t hint;
    hint["type"] = "usrp1";
    uhd::device_addrs_t dev_addrs = uhd::device::find(hint);

    -- OR --

    uhd::device_addr_t hint;
    hint["serial"] = "12345678";
    uhd::device_addrs_t dev_addrs = uhd::device::find(hint);

^^^^^^^^^^^^^^^^^
Device properties
^^^^^^^^^^^^^^^^^
Properties of devices attached to your system can be probed with the **uhd_usrp_probe** program.
This program constructs an instance of the device and prints out its properties,
such as detected daughterboards, frequency range, gain ranges, etc...

**Usage:**

::

    uhd_usrp_probe --args <device-specific-address-args>

--------------------
Naming a USRP Device
--------------------
For convenience purposes, users may assign a custom name to their USRP device.
The USRP device can then be identified via name, rather than a difficult to remember serial or address.

A name has the following properties:

* is composed of ASCII characters
* is 0-20 characters
* is not required to be unique

^^^^^^^^^^^^^^^^^
Set a custom name
^^^^^^^^^^^^^^^^^

Run the following commands:

::

    cd <install-path>/lib/uhd/utils
    ./usrp_burn_mb_eeprom --args=<optional device args> --key=name --val=lab1_xcvr

^^^^^^^^^^^^^^^^^^
Discovery via name
^^^^^^^^^^^^^^^^^^

The keyword **name** can be used to narrow the scope of the search.
Example with the find devices utility:

::

    uhd_find_devices --args="name=lab1_xcvr"

    -- OR --

    uhd_find_devices --args="type=usrp1, name=lab1_xcvr"
