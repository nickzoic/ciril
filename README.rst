===========================
 Ciril: a Cubic Inch Robot
===========================

The ESP8266 opens up new possibilities for hardware
design of educational robots.  This project aims to
put together an open hardware design compatible with
the `Flobot <http://github.com/mnemote/flobot>`_ project and also 
projects such as `NodeMCU <http://nodemcu.com/>`_.

* `Original Blog Post <http://nick.zoic.org/etc/ciril-cubic-inch-robots-in-labs/>`_

* The `EMW3165 <http://www.emw3165.com/>`_ is probably
   also worth considering ...

Why so small?
=============

The eventual aim is for dimensions of approximately
1 cubic inch (25 x 25 x 25 mm = 16 cubic centimeters),
but the immediate goal is approximately 40 x 40 x 40 mm
(4 cubic inches).

The intention is to be a size which is easy for a small 
child to pick up one-handed and which is small enough to
explore line following and so on experiments on a desktop
sized platform (approx 1m^2)

(I just noticed the `AERobot <http://affordableeducationrobot.github.io/v1.0/index.html>`_ which has very similar aims ... and instead of
wheels it shakes along on pager motors! Very strange but also
quite a cool idea)

Hardware
========

The main board features two miniature stepper motors 
soldered directly to the PCB and driving two very small 
wheels.  It is designed to run around on a desktop so 
very little ground clearance is needed.

An ESP12-E module is soldered directly to the main board
with its antenna protruding over the edge of the main board
to reduce signal interference.

A micro-USB port allows charging the built-in LiPo battery
and reflashing the ESP8266 (for Flobot or NodeMCU) via a
serial port converter.

Sensors include a line-following sensor, ambient light sensor
and an ultrasonic range sensor (if there's any pins left over,
maybe a header)

A 3D printed shell on the top and bottom of the robot provides
protection and physical support.  The lower shell supports the
motors, light guides for the line-following sensors and has skids
at either end.  The upper shell has a rounded, childs-hand-friendly
shape, with cutouts to expose the range sensor and reveal the PCB
and battery inside.

.. image:: img/render.png
    :width: 90%
    :class: center

Very much just started development!

Parts
=====

A quick manifest of parts which seem like candidates:

Processor
---------

* ESP8266 on ESP-12E module.  This has the most pins available of the
  ESP8266 modules and can be picked up for around AUD5 on Ebay.

* It is also possible that an
  `ATtiny2313 <http://www.atmel.com/images/doc2543.pdf>`_
  or similar `could communicate with the main processor <http://nick.zoic.org/etc/nodemcu-plus-plus/>`_ over serial or I2C to 
  provide more I/O.  Currently, the NodeMCU uses a USB-to-serial
  converter, for roughly the same PCB footprint we could use a more
  general piece of hardware which would be useful when the system is
  untethered.

Wheel Motors
------------

* Micro-stepper motors from Ebay in 4,6,10,15mm diameters.  
* Very small rubber wheels/tyres and also model airplane foam wheels
  worth consideration.  Experiments with foam tyres suggest they've got
  not much traction.
* Direct driving the wheels from tiny steppers is not ideal, other options
  like rubber band drive belts may need to be considered.
* this will require some experimentation.

UPDATE: The 10mm and 6mm motors have arrived and the 10mm look like
the most likely option at this point.  The 4mm motors are so tiny 
that I haven't yet worked out how to solder on to them!

Battery / Charger
-----------------

The ESP8266 runs on 3.0 - 3.6V, so 2 x AAA batteries is a possibility,
but that's a fairly large battery and AAAA don't seem to be widely 
available.

Preferably a LiIon / LiPo cell would be better. Small very high energy
ones are available for RC heli applications.  A chip like the 
`LTC3558 <http://cds.linear.com/docs/en/datasheet/3558.pdf>`_ could
act as both charge-from-USB and as an efficient LiPo -> 3.3V converter.

The little steppers seem to drive quite well on 4.5V, and potentially
could take a lot more (briefly).  So I'm also considering if the motor
circuit should run directly from two LiPo cells in series (7.2V) with
the 3.3V supply regulated by a buck regulator like LM2596.

The other thing which building a prototype made clear is that I need a
power button or switch of some kind!

Motor drivers
-------------

Driving two bipolar stepper motors is going to take 8 half H-bridges and
8 I/O pins.  It'd be great to get the pin count down by being a bit clever
about this.  The tiniest stepper motors probably draw about 50mA so there's
some room to move here ...

* `L9110S <http://www.elecrow.com/download/datasheet-l9110.pdf>`_ or
  `LV8548MC <http://www.mouser.com/ds/2/308/ENA2038-D-119504.pdf>`_ or similar. 

* Or maybe drive motors directly from a CMOS type buffer if the current
  draw is low enough.  A dual-quad-latch would reduce pin count a little. 

Line Follower
-------------

Maybe use two infrared proximity sensors such as `QRD1114 <https://www.fairchildsemi.com/datasheets/QR/QRD1114.pdf>`_.

Or maybe use two LEDs pointing down, either side of a single analog
photodiode feeding into the ADC pin.  By switching the LEDs on and off
and monitoring light level change, we can extract analog line follower
information from the single ADC.  The lower shell can provide a light guide
for these components.

Ambient Light
-------------

An LDR pointing upwards would provide a decent enough ambient light sensor 
to demonstrate phototaxis.  We've only got one ADC pin to play with but
can maybe use some output pins to choose between light sensors.

LED lighting
------------

Kids love colour.  If the shell is fairly translucent then three 
LEDs could cause it to light up nicely from the inside.  This is 
also good as a diagnostic output.  Of course, that's 3 more PWM channels ...

Proximity Sensor
----------------

There are `heaps of modules around <http://www.ebay.com.au/sch/i.html?_nkw=ultrasonic+module>`_ which use a pair of ultrasonic
transducers, one to transmit and one to receive.  However, we should be
able to do better and use a single device with clever driver software to
switch from transmit to receive.  Accuracy isn't that important so long
as we can detect a barrier.

Alternatives are the `Sharp Infrared distance sensors <http://www.sharpsma.com/webfm_send/1489>`_ or similar.

Photos
======

.. image:: img/teenyrobots.jpg
    :width: 90%
    :class: center

.. image:: img/ciril-proto.jpg
    :width: 90%
    :class: center

.. image:: img/ciril-wheels.png
    :width: 90%
    :class: center

