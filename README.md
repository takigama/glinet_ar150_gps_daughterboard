## What be it?

Simple daughter board for the AR150 from GL.inet using the Quectel L80 GPS

## Why?

meh, why not?

Seriously though, some time ago I used the predessesor of the AR150 and the
uBlox Neo 6m GPS receiver to build a NTP with GPS and PPS for high accuracy
timing. [See here](http://pjrlost.blogspot.com.au/2015/09/ar9331-pps-gps-ntp-awesome.html)
I had to write some code to enable PPS interupt based processing on the AR9331
family (thanks to some code that already supported GPIO's with interrupts)
and was fairly impressed with the results.

Sadly its hard to get the Neo uBlox 6M in an SMD format, but I stumbled onto
the Quectel L80 chip, which is easy to aquire and even smaller than the uBlox. The AR150
has different GPIO pins to the original GL.inet router I had in my previous
project (which is still actually running at home and still out-performing
most other GPS/PPS's i've built).

A number of projects do use the AR9331 as a GPS/PPS/NTP clock, but this was
the first one that I know of that implemented by the gpio interrupt mechanism
(which is more accurate and more efficient).

## How?

Well, first go print a board at oshpark [here](https://oshpark.com/shared_projects/Jt3Wxv2q)
and then go get a Quectel L80 chip (just the chip, no board), a couple of leds
a small mosfet (dont really need it to be honest) and a few resistors and lastly
of course a AR150.

Solder it all together, then sit back and wait for me to port my code on the
blog to the AR150 (only have to change the PPS pin from 13 to 14), or compile
the code yourself and figure that bit out.

Also, when i started this, i was thinking along the lines of the PoE version
of the AR150 (so the AR150 can sit outside in a silicon case with the GPS poking
upward thru the top - i'll post pic's when I build it)

Dont let the SMD scare you, plenty of youtubes up for how to hand solder or heat
gun solder this thing. I use a head gun for everything except the GPS module. I
use a soldering iron for the GPS module.

## BOM

1) 1 x L80 GPS Module or MTK3339 Module
2) 1 x Mosfet (BSS138 or similar) - optional, allows GPS to be power cycled by board
3) 3 x 200ohm 0805 SMD resistors
4) 1 x 2x5 90 degree angled pin header
5) 3 x 0805 leds (colour is up to you)


## GPIO's

So in the project, its using two GPIO's. One is used to turn the GPS itself on
and off. The other is used for the PPS interrupt. GPIO 17 is the power switch
GPIO 14 will be the PPS pin.

## Warning!

This isnt very mature yet. I've tested the L80 board, and have since updated
its layout. It works ok, though getting the firmware for the AR150 going was
a tedious excersize.

I have not however tested the MTK3339 based board....

## Updates

I've printed the first iteration and mounted it to the board. Didnt quite
get all the sizing right, so now there is version 0.1b. Stay tuned

Added an MTK3339 GPS board layout (same sized footprint as the L80) partly
because the L80 seems harder to find than the MTK and the performance of the
L80 hasnt been spectacular so far (in terms of sensitivity).
