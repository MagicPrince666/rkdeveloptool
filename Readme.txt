rkdeveloptool gives you a simple way to read/write rockusb device.let's start.

compile and install
1 install libusb and libudev
	sudo apt-get install libudev-dev libusb-1.0-0-dev dh-autoreconf
2 go into root of rkdeveloptool
3.aclocal
4.autoreconf -i
5.autoheader
5.automake --add-missing
4 ./configure
5 make

rkdeveloptool usage,input "rkdeveloptool -h" to see

example:
1.download kernel.img
./rkdeveloptool ld        # List the device
./rkdeveloptool db rk3308_loader_uart0_m0_emmc_port_support_sd_20190717.bin
./rkdeveloptool wl 0 openwrt-rockchip-armv8-radxa_rock-pi-s-squashfs-sysupgrade.img //0x8000 is base of kernel partition,unit is sector.
./rkdeveloptool rd                   //reset device

compile error help
if you encounter the error like below:
./configure: line 4269: syntax error near unexpected token `LIBUSB1,libusb-1.0'
./configure: line 4269: `PKG_CHECK_MODULES(LIBUSB1,libusb-1.0)'

You should install pkg-config libusb-1.0:
	sudo apt-get install pkg-config libusb-1.0 

gzip -d openwrt-rockchip-armv8-radxa_rock-pi-s-squashfs-sysupgrade.img.gz

mmc list
mmc dev 1  //切换到 SD 卡，0 为 SD 卡，1 为 eMMC

mmc info