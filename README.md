create usb with rufus using this file

https://downloads.openwrt.org/releases/19.07.4/targets/x86/64/openwrt-19.07.4-x86-64-combined-ext4.img.gz

after boot from usb

opkg update
opkg install libustream-openssl

cd /tmp

wget https://downloads.openwrt.org/releases/19.07.4/targets/x86/64/openwrt-19.07.4-x86-64-combined-ext4.img.gz --no-check-certificate

gunzip openwrt-19.07.4-x86-64-combined-ext4.img.gz

dd if=openwrt-19.07.4-x86-64-combined-ext4.img of=/dev/sda bs=4M; sync;

returns:

=4M; sync;
68+1 records in
68+1 records out

opkg update && opkg install cfdisk resize2fs

cfdisk
e2fsck -f /dev/sda2
resize2fs /dev/sda2

turn off
remove usb 
turn on

install APU specific packages

opkg update && opkg install kmod-leds-apu2 kmod-leds-gpio kmod-crypto-hw-ccp kmod-gpio-nct5104d kmod-gpio-button-hotplug kmod-sp5100_tco kmod-usb-core kmod-usb-ohci kmod-usb2 kmod-usb3 kmod-sound-core kmod-pcspkr  amd64-microcode flashrom irqbalance fstrim

opkg update && opkg install minidlna luci-app-minidlna

Syncing disks.
root@OpenWrt:/# resize2fs /dev/sda2
resize2fs 1.44.5 (15-Dec-2018)
Please run 'e2fsck -f /dev/sda2' first.

root@OpenWrt:/# e2fsck -f /dev/sda2
e2fsck 1.44.5 (15-Dec-2018)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
Free blocks count wrong (61330, counted=61323).
Fix<y>? yes
Free inodes count wrong (15149, counted=15091).
Fix<y>? yes

/dev/sda2: ***** FILE SYSTEM WAS MODIFIED *****
/dev/sda2: 1293/16384 files (0.0% non-contiguous), 4213/65536 blocks
root@OpenWrt:/# resize2fs /dev/sda2
resize2fs 1.44.5 (15-Dec-2018)
Resizing the filesystem on /dev/sda2 to 29300854 (4k) blocks.
The filesystem on /dev/sda2 is now 29300854 (4k) blocks long.


uci set network.lan.ipaddr=

uci -q delete system.ntp.server
uci add_list system.ntp.server="0.ch.pool.ntp.org"
uci add_list system.ntp.server="1.ch.pool.ntp.org"
uci add_list system.ntp.server="2.ch.pool.ntp.org"
uci add_list system.ntp.server="3.ch.pool.ntp.org"
uci commit system
/etc/init.d/sysntpd restart
