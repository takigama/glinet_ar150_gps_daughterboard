# LEDE Patch for AR150 support

quick and nasty:

1. this is a hack
2. this IS a hack
3. hard codes the pps-gpio module to the GL-AR150
4. includes the configuration for building lede
5. follow https://openwrt.org/docs/guide-developer/quickstart-build-images for building lede
6. copy the lede patch 940-lede-patch-for-ar150-pps-gpio.patch to target/linux/ar71xx/patches-4.9/

Build it all!

