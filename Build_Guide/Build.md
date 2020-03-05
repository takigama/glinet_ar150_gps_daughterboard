# Quick Run Through

## And then there was light

1. git clone https://git.openwrt.org/openwrt/openwrt.git
2. cd openwrt
3. git checkout v19.07.2
4. ./scripts/feeds update -a
5. ./scripts/feeds install -a
6. copy config.built to ./.config
7. copy 940-lede-patch-for-ar150-pps-gpio.patch to target/linux/ar71xx/patches-4.14/
8. make menuconfig (adjust as you need)
9. make
10. Cross fingers
11. firmware should now be in bin/targets/ar71xx/generic/
