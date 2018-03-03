# New firmwares - 3/Mar/2018

This new firmware is now pretty solid and there are two versions

1. LEDE based (based on lede github master)
2. openwrt based (comes from openwrt's own master git)

## Details

There are quite alof ot packages added, which i'll update shortly
but to install it, upload it to your AR150 (preferably already 
running openwrt) then run from the shell "sysupgrade -v /tmp/my_firmware_file"

Once you've done that, its a good idea to blank it when it comes up
by running "mtd -r erase rootfs_data" (this will reset the router
back to the golden image and reboot it).

On boot it will do a number of things:

1. set WAN interface to DHCP client
2. set wifi SSID to "OpenWRT" and give the wifi a 192.168.254.1 with dhcp server
3. set LAN interface to 192.168.253.1 with a dhcp server
4. enabble gpsd and chrony with two ntp servers on "noselect"
5. disables firewall!!!!
6. disables most every other service
7. disables the serial console

The reason we do all that is to remove as much cpu-consuming tasks from
the router as possible, but it is obviously insecure. If you dont need it
disable the wifi, but I would leave the firewall off, configure a static
IP and shutdown the web interface, and the "LAN" interface. On the PoE
model, the WAN interface is the one that accepts PoE and if your using 
an outdoor case (coming soon(tm)), you'll only want a single ethernet
cable coming into the router.

Once its online and you can connect to it (either by plugging LAN into
your network, connecting to the SSID or connecting to the LAN interface)
ssh in and run "chronyc sources", it should start syncing. You can also
make sure gpsd is running correctly with cgps (you should see a 9600 baud
rate on the serial line)

One problem - for some reason after a sysupgrade or mtd erase step the wifi
does not come back online unless the router is rebooted again. So after
upgrading and resetting, make sure you reboot if you need wireless!

I'll make the firmwares available soon on githuba

If your not sure which firmware you installed, have a look in /etc/taki
and there will be a 0 byte file that it is either "lede" or "operwrt".

## PTP

This version has full ptp support, but as yet does not actually start the
ptp daemon. If you wish to test that functionality, justs login to the router
and run "ptp4l -i eth0 -q -m -A -S" (note that'll tie itself to the console,
so you'd have to setup ptp after this step if its whats you need.

## NTPD instead of Chrony.

My firmware defaults to starting up chrony. If you wish to use ntpd instead
just disable the chrony startup and enable the ntp one. Then configure
the ntpd.
