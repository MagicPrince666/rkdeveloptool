rkdeveloptool gives you a simple way to read/write rockusb device.let's start.

compile and install
1 install libusb and libudev
	sudo apt-get install libudev-dev libusb-1.0-0-dev dh-autoreconf
	brew install automake
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


CONFIG_RFKILL_FULL=y
CONFIG_BT=y
CONFIG_BT_HCIUART=y
CONFIG_BT_BREDR=y
CONFIG_BT_RFCOMM=y
CONFIG_BT_HS=y
CONFIG_BT_LE=y

mmc info

part uuid mmc 1:2 uuid
setenv bootargs "console=ttyS0,1500000 earlycon=uart8250,mmio32,0xff0a0000 root=PARTUUID=5452574f-02 rw rootwait"
load mmc 1:1 0x01f00000 rockchip/rk3308-rock-pi-s.dtb
load mmc 1:1 0x00280000 kernel.img
booti 0x00280000 - 0x01f00000

固件下载
```
./rkdeveloptool db rockdev/rk3308_loader_v2.05.132.bin
./rkdeveloptool wl 0 rockdev/parameter.txt
./rkdeveloptool wl 0x2000 rockdev/uboot.img
./rkdeveloptool wl 0x3000 rockdev/trust.img
./rkdeveloptool wl 0x4000 rockdev/misc.img
./rkdeveloptool wl 0xA800 rockdev/boot.img
./rkdeveloptool wl 0xF000 rockdev/rootfs.img
./rkdeveloptool wl 0x25000 rockdev/oem.img
./rkdeveloptool wl 0x3D000 rockdev/userdata.img
./rkdeveloptool rd
```

原版固件下载
```
./rkdeveloptool db ~/cbox/rockdev/rk3308_loader_v2.05.132.bin
./rkdeveloptool wl 0 ~/cbox/rockdev/parameter-64bit-emmc.txt
./rkdeveloptool wl 0x2000 ~/cbox/rockdev/uboot.img
./rkdeveloptool wl 0x3000 ~/cbox/rockdev/trust.img
./rkdeveloptool wl 0x4000 ~/cbox/rockdev/misc.img
./rkdeveloptool wl 0xA800 ~/cbox/rockdev/boot.img
./rkdeveloptool wl 0xF000 ~/cbox/rockdev/rootfs.img
./rkdeveloptool wl 0x25000 ~/cbox/rockdev/oem.img
./rkdeveloptool wl 0x3D000 ~/cbox/rockdev/userdata.img
./rkdeveloptool rd
```
