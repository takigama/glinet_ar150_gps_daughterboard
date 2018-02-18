# New firmware - 18/Feb/2018

This firmware is something of a hack, but is running lede.

Has all of the components necessary to support an gps/pps machine

1. ntp
2. chrony
3. linuxptp
4. picocom
5. a bunch of extra components...

This is hardcoded for gps/pps on gpio 14

With my startup, i disable most of the ntp clients and start things
up with /etc/rc.local:

```
# Put your custom commands here that should be executed once
# the system init finished. By default this file does nothing.


setserial /dev/ttyATH0 low_latency 
stty -F /dev/ttyATH0 9600 

echo 17 > /sys/class/gpio/export
echo out > /sys/class/gpio/gpio17/direction
echo 1 > /sys/class/gpio/gpio17/value


/bin/ln -s /dev/ttyATH0 /dev/gps0
/bin/ln -s /dev/pps0 /dev/gpspps0
# mkdir -p /var/log/ntpstats
# chown ntp /var/log/ntpstats
# sleep 5
# /etc/init.d/ntpd restart
/usr/sbin/gpsd -n -G -S 2947 /dev/ttyATH0 >> /tmp/err.log

```

Note that by default gpio 17 is low, so the gps is switchede off
by default and we have to turn it on at some point.
