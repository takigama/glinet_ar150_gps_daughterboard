# New firmware - 18/Feb/2018

This firmware is something of a hack, but is running lede.

Has all of the components necessary to support an gps/pps machine

1. ntp
2. chrony
3. linuxptp
4. picocom
5. a bunch of extra components...

This is hardcoded for gps/pps on gpio 14 - also note thata if you
are using the board from this repo IT WILL BE OFF BY DEFAULT!!!!
you must turn on gpio 17 for the board to come alive

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

Heres how i set up my routers after the firmware is installed (you
should disable most of these, and my setup uses chrony where you may
with to use ntpd. Also i disable the firewall - thats up to you to
decide if you want that or not):

1. disable some daemons
--1. /etc/init.d/htpdate disable
--2. /etc/init.d/gpsd disable
--3. /etc/init.d/firewall disable
--4. /etc/init.d/sysntpd disable
--5. /etc/init.d/ntpd disable
2. Configure Chrony - see below
2. Alternatively, configure NTPD, i'll provide one shortly
3. check pps is living (you should see one of the leds flashing
on the routers daughter boarrd if it is) with ppstest /dev/pps0
4. Fix /etc/init.d/rc.local as per above
5. DISABLE the lan interface bridge (read below)
6. configure linuxptp (config coming soon)


## Chrony Config

### /etc/chrony/chrony.conf  
```
# Chrony configuration

# Note: time servers and ntp client access is configured in /etc/config/ntpd 
# and automatically set at startup


# Log clock errors above 0.5 seconds
logchange 0.5

# Allow command access only from localhost
cmdallow localhost
cmddeny

logdir /tmp/

log measurements statistics tracking


# Password config for chronyc
# Note: Using a command key other than "1" will break 
# /etc/init.d/ntpd and /etc/hotplug.d/iface/20-ntpd
keyfile /etc/chrony/chrony.keys
commandkey 1

refclock SHM 0 offset 0.09 refid NMEA noselect
refclock PPS /dev/pps0 refid PPS lock NMEA


allow all

# give yourself a reference to see how your tracking
server 0.pool.ntp.org noselect 
server 1.pool.ntp.org noselect
server 2.pool.ntp.org noselect
server 3.pool.ntp.org noselect


```

## Networking

If you wish to use the linuxptp daemon, you have to disable the network bridge

### /etc/config/network

note that I only use eth0 here, the AR150 (and ar9331 in general) has two ports,
eth0 which is a standalone phy and eth1 which is a phy connected to the internal
switch. The AR150 uses PoE on eth0 and eth0 is also a slightly better choice to 
connect to as it has not switch to introduce latency. We also remove the bridge
because linuxPTP cannot use the bridge interface

```

config interface 'loopback'
        option ifname 'lo'
        option proto 'static'
        option ipaddr '127.0.0.1'
        option netmask '255.0.0.0'

config globals 'globals'
        option ula_prefix 'fd00:a76f:e7bc::/48'

config interface 'lan'
        option ifname 'eth0'
        option proto 'dhcp'


```

And thats pretty much it for now....
