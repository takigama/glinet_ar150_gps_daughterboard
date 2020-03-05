## Details for building openwrt for this project...

The quick and dirty:
git clone https://github.com/openwrt/openwrt.git
git checkout d95f4c6ca377edcdf8c827a50b5e67e12313f9f6
git reset --hard # just incase

copy the 971 patch in this dir to target/linux/ar71xx/patches-4.1/ in your openwrt directory
copy the config in this directory to the root of your openwrt directory

Hope that it all builds. Theres one file that isnt downloaded during the build called
DPO_MT7601U_LinuxSTA_3.0.0.4_20130913.tar.bz2 which can be found via google. Download
that and it goes into your openwrt under a directory called dl/

upgrade your ar150 with the firmware you finially get out of it.

Or just upload the firmware in the openwrt-firmware directory.
