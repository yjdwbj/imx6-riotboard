# RIotBoard
## Product Introduction
The RIoTboard is an evaluation platform featuring the powerful i.MX 6Solo, a
multimedia application processor with ARM Cortex-A9 core at 1 GHz from Freescale
Semiconductor. The platform helps evaluate the rich set of peripherals and includes
a 10/100/Gb Ethernet port, HDMI v1.4, LVDS, analog headphone/microphone, uSD
and SD card interface, USB, serial port, JTAG, 2 camera interfaces, GPIO boot
configuration interface, and expansion port.

Releases:
* [Kernel 6.8.6](#kernel-6.8.6)
* [u-boot - 2018.05 ](#u-boot)
* [Debian 12(bookworm)](https://www.debian.org/News/2023/20230610)

## U-boot

Uboot Build instructions.

* Install dependencies

```sh
    sudo apt-get install device-tree-compiler gcc-arm-linux-gnueabi
```

* Build and Install u-boot (SD/MMC card)

```sh
    cd u-boot-2018.05
    ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- make riotboard_config
    ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- make -j $(nproc)
    dd if=u-boot.imx of=/dev/sdb bs=1024 seek=1

```

* Pre-built

```sh
    https://github.com/yjdwbj/imx6-riotboard/archive/refs/tags/v1.0.tar.gz
```


## Kernel 6.8.6

Kernel 6.8.6 is rock solid in RIotBoard, building **Kernel 6.8.6** with gcc-12 on uSD for testing new devfreq and checking stability.
The board works great in real time using Gstreamer and a hardware h264 encoder.

![webrtc](https://github.com/yjdwbj/imx6-riotboard/raw/main/webrtc-sendonly-htop.png)


Testing mainline **6.8.6**, current status:

* Eth0 - ok
* Wifi Realtek 8812AU/8821AU (5 GHz) - ok
* Wifi mt7601u (2.4 GHz) - ok
* eMMC - ok
* hdmi - ok (for debugging)
* sound - JACK output and HDMI output (ok)
* USB2 - ok
* fbdev - ok (for ili9341 spi)

## Benchmarks

* CPU info

```sh
riot@bullseye:~$ lscpu
Architecture:                       armv7l
Byte Order:                         Little Endian
CPU(s):                             1
On-line CPU(s) list:                0
Thread(s) per core:                 1
Core(s) per socket:                 1
Socket(s):                          1
Vendor ID:                          ARM
Model:                              10
Model name:                         Cortex-A9
Stepping:                           r2p10
CPU max MHz:                        996.0000
CPU min MHz:                        396.0000
BogoMIPS:                           6.00
Vulnerability Gather data sampling: Not affected
Vulnerability Itlb multihit:        Not affected
Vulnerability L1tf:                 Not affected
Vulnerability Mds:                  Not affected
Vulnerability Meltdown:             Not affected
Vulnerability Mmio stale data:      Not affected
Vulnerability Retbleed:             Not affected
Vulnerability Spec rstack overflow: Not affected
Vulnerability Spec store bypass:    Not affected
Vulnerability Spectre v1:           Mitigation; __user pointer sanitization
Vulnerability Spectre v2:           Mitigation; Branch predictor hardening
Vulnerability Srbds:                Not affected
Vulnerability Tsx async abort:      Not affected
Flags:                              half thumb fastmult vfp edsp neon vfpv3 tls
                                    vfpd32
riot@bullseye:~$ cat /proc/cpuinfo
processor       : 0
model name      : ARMv7 Processor rev 10 (v7l)
BogoMIPS        : 6.00
Features        : half thumb fastmult vfp edsp neon vfpv3 tls vfpd32
CPU implementer : 0x41
CPU architecture: 7
CPU variant     : 0x2
CPU part        : 0xc09
CPU revision    : 10

Hardware        : Freescale i.MX6 Quad/DualLite (Device Tree)
Revision        : 0000
Serial          : 0000000000000000

```

* login motd

```sh
____  ___    _____ _                         _
|  _ \|_ _|__|_   _| |__   ___   __ _ _ __ __| |
| |_) || |/ _ \| | | '_ \ / _ \ / _` | '__/ _` |
|  _ < | | (_) | | | |_) | (_) | (_| | | | (_| |
|_| \_\___\___/|_| |_.__/ \___/ \__,_|_|  \__,_|

Welcome to Debian GNU/Linux 12 (bookworm) with Linux 6.8.6-riotboard

System load:   18%           	Up time:       7 min
Memory usage:  9% of 567M   	IP:	       192.168.1.150
CPU temp:      57°C
RX today:      367.2 MiB

Last login: Sun Apr 14 20:51:18 2024 from 192.168.1.182

```

* 7zr b
```sh
riot@bullseye:~$ 7zr b

7-Zip (a) [32] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_US.UTF-8,Utf16=on,HugeFiles=on,32 bits,2 CPUs LE)

LE
CPU Freq: 32000000 21333333 12800000 32000000 64000000 85333333 128000000 341333333 682666666

RAM size:     981 MB,  # CPU hardware threads:   2
RAM usage:    441 MB,  # Benchmark threads:      2

                       Compressing  |                  Decompressing
Dict     Speed Usage    R/U Rating  |      Speed Usage    R/U Rating
         KiB/s     %   MIPS   MIPS  |      KiB/s     %   MIPS   MIPS

22:        504    95    515    491  |       8908    96    796    761
23:        482    95    515    492  |       8730    96    791    756
24:        469    96    528    505  |       8535    95    785    749
25:        458    96    548    523  |       8331    95    777    742
----------------------------------  | ------------------------------
Avr:              95    526    503  |               95    787    752
Tot:              95    657    627

```

* openssl (crypto)

```sh
riot@bullseye:~$ openssl speed -elapsed -evp aes-128-gcm aes-128-cbc sha256
You have chosen to measure elapsed time instead of user CPU time.
Doing sha256 for 3s on 16 size blocks: 1837204 sha256's in 3.00s
Doing sha256 for 3s on 64 size blocks: 1094295 sha256's in 3.00s
Doing sha256 for 3s on 256 size blocks: 496300 sha256's in 3.00s
Doing sha256 for 3s on 1024 size blocks: 160567 sha256's in 3.00s
Doing sha256 for 3s on 8192 size blocks: 22082 sha256's in 3.00s
Doing sha256 for 3s on 16384 size blocks: 10731 sha256's in 3.00s
Doing aes-128 cbc for 3s on 16 size blocks: 5520821 aes-128 cbc's in 3.00s
Doing aes-128 cbc for 3s on 64 size blocks: 1536268 aes-128 cbc's in 3.00s
Doing aes-128 cbc for 3s on 256 size blocks: 390363 aes-128 cbc's in 3.00s
Doing aes-128 cbc for 3s on 1024 size blocks: 101149 aes-128 cbc's in 3.00s
Doing aes-128 cbc for 3s on 8192 size blocks: 12752 aes-128 cbc's in 3.00s
Doing aes-128 cbc for 3s on 16384 size blocks: 6176 aes-128 cbc's in 3.00s
Doing aes-128-gcm for 3s on 16 size blocks: 3363172 aes-128-gcm's in 3.00s
Doing aes-128-gcm for 3s on 64 size blocks: 1098607 aes-128-gcm's in 3.00s
Doing aes-128-gcm for 3s on 256 size blocks: 299680 aes-128-gcm's in 3.00s
Doing aes-128-gcm for 3s on 1024 size blocks: 84672 aes-128-gcm's in 3.00s
Doing aes-128-gcm for 3s on 8192 size blocks: 10972 aes-128-gcm's in 3.00s
Doing aes-128-gcm for 3s on 16384 size blocks: 5305 aes-128-gcm's in 3.00s
OpenSSL 1.1.1w  11 Sep 2023
built on: Wed Sep 13 19:21:33 2023 UTC
options:bn(64,32) rc4(char) des(long) aes(partial) blowfish(ptr)
compiler: gcc -fPIC -pthread -Wa,--noexecstack -Wall -Wa,--noexecstack -g -O2 -ffile-prefix-map=/build/reproducible-path/openssl-1.1.1w=. -fstack-protector-strong -Wformat -Werror=format-security -DOPENSSL_USE_NODELETE -DOPENSSL_P2
The 'numbers' are in 1000s of bytes per second processed.
type             16 bytes     64 bytes    256 bytes   1024 bytes   8192 bytes  16384 bytes
aes-128 cbc      29444.38k    32773.72k    33310.98k    34525.53k    34821.46k    33729.19k
aes-128-gcm      17936.92k    23436.95k    25572.69k    28901.38k    29960.87k    28972.37k
sha256            9798.42k    23344.96k    42350.93k    54806.87k    60298.58k    58605.57k

```

## Networking (Wifi / Eth0 )

```sh
riot@bullseye:~$ lsusb
Bus 001 Device 004: ID 148f:7601 Ralink Technology, Corp. MT7601U Wireless Adapter
Bus 001 Device 003: ID 0bda:0811 Realtek Semiconductor Corp. Realtek 8812AU/8821AU 802.11ac WLAN Adapter [USB Wireless Dual-Band Adapter 2.4/5Ghz]
Bus 001 Device 002: ID 1a40:0101 Terminus Technology Inc. Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

riot@bullseye:~$ brctl show
bridge name	bridge id		STP enabled	interfaces
br-lan		8000.061f332cd6df	no		eth0
							wlan0
riot@bullseye:~$ iw wlan0 info
Interface wlan0
	ifindex 5
	wdev 0x1
	addr e8:4e:06:5b:fa:98
	ssid homecreate
	type AP
	wiphy 0
	channel 44 (5220 MHz), width: 40 MHz, center1: 5230 MHz
	txpower 20.00 dBm
riot@bullseye:~$ iw wlan1 info
Interface wlan1
	ifindex 9
	wdev 0x200000001
	addr 64:fb:81:65:b6:dc
	type managed
	wiphy 2
	txpower 20.00 dBm
	multicast TXQ:
		qsz-byt	qsz-pkt	flows	drops	marks	overlmt	hashcol	tx-bytes	tx-packets
		0	0	0	0	0	0	0	0		0

```

## tailscale

```sh
riot@bullseye:~$ ifconfig tailscale0
tailscale0: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST>  mtu 1280
        inet6 fe80::3d09:9c12:8d29:2318  prefixlen 64  scopeid 0x20<link>
        unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen 500  (UNSPEC)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 11  bytes 528 (528.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

## Jack

```sh
riot@bullseye:~$ play Deepside-BootyMusic.mp3
play WARN alsa: can't encode 0-bit Unknown or not applicable

Deepside-BootyMusic.mp3:

 File Size: 7.29M     Bit Rate: 321k
  Encoding: MPEG audio    Info: 2008
  Channels: 2 @ 16-bit
Samplerate: 44100Hz      Album: www.RnB4U.in
Replaygain: off         Artist: Deepside - Booty Music (2008 CDQ Shoutless) [www.RnB4U.in]
  Duration: 00:03:01.79  Title: Deepside - Booty Music (2008 CDQ Shoutless) [www.RnB4U.in]

In:4.60% 00:00:08.36 [00:02:53.43] Out:369k  [    -=|=     ]        Clip:0


```

## HDMI

```sh
riot@bullseye:~$ AUDIODEV=hw:0,0 play Deepside-BootyMusic.mp3
play WARN alsa: can't encode 0-bit Unknown or not applicable

Deepside-BootyMusic.mp3:

 File Size: 7.29M     Bit Rate: 321k
  Encoding: MPEG audio    Info: 2008
  Channels: 2 @ 16-bit
Samplerate: 44100Hz      Album: www.RnB4U.in
Replaygain: off         Artist: Deepside - Booty Music (2008 CDQ Shoutless) [www.RnB4U.in]
  Duration: 00:03:01.79  Title: Deepside - Booty Music (2008 CDQ Shoutless) [www.RnB4U.in]

In:0.92% 00:00:01.67 [00:03:00.12] Out:73.7k [   ===|==-   ]        Clip:0
```

## Graphics Testing

### DT Overlay for dual ILI9341 LCD testing

* DT Overlay

```sh
/dts-v1/;
/plugin/;

#include <dt-bindings/gpio/gpio.h>

/ {
	compatible = "riot,imx6s-riotboard", "fsl,imx6dl";
	fragment@1 {
		target = <&ecspi2>;
		__overlay__ {
			    pinctrl-names = "default";
	            pinctrl-0 = <&pinctrl_ecspi2>;
	            status = "okay";
                #address-cells = <1>;
                #size-cells = <0>;
                cs-gpios = <&gpio5 12 GPIO_ACTIVE_LOW>, <&gpio5 9 GPIO_ACTIVE_LOW>;

                ili9341@0 {
                    rotate = <90>;
                    bgr;
                    fps = <30>;
                    compatible = "ilitek,ili9341", "spidev";
                    spi-max-frequency = <50000000>;
                    reg = <0>;
                    buswidth = <8>;
                    verbose = <0>;
                    reset-gpios = <&gpio4 21 GPIO_ACTIVE_LOW>;
                    dc-gpios = <&gpio4 22 GPIO_ACTIVE_HIGH>;
                };

                ili9341@1 {
                    rotate = <270>;
                    bgr;
                    fps = <30>;
                    compatible = "ilitek,ili9341", "spidev";
                    spi-max-frequency = <50000000>;
                    reg = <1>;
                    buswidth = <8>;
                    verbose = <0>;
                    reset-gpios = <&gpio4 23 GPIO_ACTIVE_LOW>;
                    dc-gpios = <&gpio4 24 GPIO_ACTIVE_HIGH>;
                };
		};
	};
};
```

* DT plugin debug

```sh
[  601.366716] fbtft: module is from the staging directory, the quality is unknown, you have been warned.
[  601.388959] fb_ili9341: module is from the staging directory, the quality is unknown, you have been warned.
[  601.404035] fb_ili9341 spi1.1: fbtft_property_value: buswidth = 8
[  601.410487] fb_ili9341 spi1.1: fbtft_property_value: rotate = 270
[  601.416844] fb_ili9341 spi1.1: fbtft_property_value: fps = 30
[  601.691690] imx-sdma 20ec000.dma-controller: sdma firmware not ready!
[  601.790739] Console: switching to colour frame buffer device 40x30
[  601.815346] graphics fb0: fb_ili9341 frame buffer, 320x240, 150 KiB video memory, 16 KiB buffer memory, fps=30, spi1.1 at 50 MHz
[  601.832255] fb_ili9341 spi1.0: fbtft_property_value: buswidth = 8
[  601.839879] fb_ili9341 spi1.0: fbtft_property_value: rotate = 90
[  601.847377] fb_ili9341 spi1.0: fbtft_property_value: fps = 30
[  602.373174] graphics fb1: fb_ili9341 frame buffer, 320x240, 150 KiB video memory, 16 KiB buffer memory, fps=30, spi1.0 at 50 MHz

```

### LVGL testing

* Demo 1

```sh
#include "lvgl/lvgl.h"
#include "lvgl/demos/lv_demos.h"
#include <unistd.h>
#include <pthread.h>
#include <time.h>

int main(void)
{
    lv_init();

    /*Linux frame buffer device init*/
    lv_display_t * disp = lv_linux_fbdev_create();
    lv_linux_fbdev_set_file(disp, "/dev/fb0");

    /*Create a Demo*/
    lv_demo_widgets();
    lv_demo_widgets_start_slideshow();

    /*Handle LVGL tasks*/
    while(1) {
        lv_timer_handler();
        usleep(5000);
    }

    return 0;
}

```

* Benchmark Demo

```sh
#include "lvgl/lvgl.h"
#include "lvgl/demos/lv_demos.h"
#include <unistd.h>
#include <pthread.h>
#include <time.h>

int main(void)
{
    lv_init();

    /*Linux frame buffer device init*/
    lv_display_t * disp = lv_linux_fbdev_create();
    lv_linux_fbdev_set_file(disp, "/dev/fb1");

    /*Create a Demo*/
    lv_demo_benchmark();

    /*Handle LVGL tasks*/
    while(1) {
        lv_timer_handler();
        usleep(5000);
    }

    return 0;

```

![lvgl-test-bench.png](images/lvgl-test-bench.png)
![dual-ili9341-lvgl-bench.gif](images/dual-ili9341-lvgl-bench.gif)


### Qt5 testing

* Run Qt5 example of player to `/dev/fb0` in 2.8 inch LCD.

```sh
/usr/lib/arm-linux-gnueabihf/qt5/examples/multimediawidgets/player/player -platform linuxfb:fb=/dev/fb0
QStandardPaths: XDG_RUNTIME_DIR not set, defaulting to '/tmp/runtime-root'
Supported audio roles:

```

* Run Qt5 example of spectrum to `/dev/fb1` in 2.4 inch LCD.

```sh
/usr/lib/arm-linux-gnueabihf/qt5/examples/multimedia/spectrum/spectrum -platform linuxfb:fb=/dev/fb1
QStandardPaths: XDG_RUNTIME_DIR not set, defaulting to '/tmp/runtime-root'
PulseAudioService: pa_context_connect() failed
Engine::initialize frequenciesList ()
Engine::initialize channelsList ()
Engine::initialize m_bufferLength 0
Engine::initialize m_dataLength 0
Engine::initialize format QAudioFormat(-1Hz, -1bit, channelCount=-1, sampleType=Unknown, byteOrder=LittleEndian, codec="")
Engine::initialize m_audioOutputCategory ""
libpng warning: iCCP: known incorrect sRGB profile
```

![qt5-testing.png](images/qt5-testing.png)

## Boot log (booting from uSD Card)

```sh
[    0.000000] Booting Linux on physical CPU 0x0
[    0.000000] Linux version 6.8.6-riotboard (yjdwbj@gmail.com) (arm-linux-gnueabi-gcc (Debian 12.2.0-14) 12.2.0, GNU ld (GNU Binutils for Debian) 2.40) #4 SMP Sun Apr 14 19:24:26 CST 2024
[    0.000000] CPU: ARMv7 Processor [412fc09a] revision 10 (ARMv7), cr=10c5387d
[    0.000000] CPU: PIPT / VIPT nonaliasing data cache, VIPT aliasing instruction cache
[    0.000000] OF: fdt: Machine model: RIoTboard i.MX6S
[    0.000000] Memory policy: Data cache writeback
[    0.000000] Ignoring RAM at 0x36000000-0x50000000
[    0.000000] Consider using a HIGHMEM enabled kernel.
[    0.000000] cma: Reserved 128 MiB at 0x2e000000 on node -1
[    0.000000] Zone ranges:
[    0.000000]   Normal   [mem 0x0000000010000000-0x0000000035ffffff]
[    0.000000] Movable zone start for each node
[    0.000000] Early memory node ranges
[    0.000000]   node   0: [mem 0x0000000010000000-0x0000000035ffffff]
[    0.000000] Initmem setup node 0 [mem 0x0000000010000000-0x0000000035ffffff]
[    0.000000] CPU: All CPU(s) started in SVC mode.
[    0.000000] percpu: Embedded 19 pages/cpu s45108 r8192 d24524 u77824
[    0.000000] pcpu-alloc: s45108 r8192 d24524 u77824 alloc=19*4096
[    0.000000] pcpu-alloc: [0] 0 [0] 1
[    0.000000] Kernel command line: root=/dev/mmcblk2p2 rootwait rootfstype=ext4 ttymxc1,115200 nosmp video=mxcfb0:dev=hdmi,1280x720M@60,bpp=32 net.ifnames=0 video=mxcfb1:off fbmem=10M vmalloc=400M
[    0.000000] Unknown kernel command line parameters "ttymxc1,115200 fbmem=10M", will be passed to user space.
[    0.000000] Dentry cache hash table entries: 131072 (order: 7, 524288 bytes, linear)
[    0.000000] Inode-cache hash table entries: 65536 (order: 6, 262144 bytes, linear)
[    0.000000] Built 1 zonelists, mobility grouping on.  Total pages: 154432
[    0.000000] mem auto-init: stack:all(zero), heap alloc:off, heap free:off
[    0.000000] Memory: 449408K/622592K available (18432K kernel code, 2459K rwdata, 6288K rodata, 1024K init, 6735K bss, 42112K reserved, 131072K cma-reserved)
[    0.000000] SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=2, Nodes=1
[    0.000000] trace event string verifier disabled
[    0.000000] Running RCU self tests
[    0.000000] Running RCU synchronous self tests
[    0.000000] rcu: Hierarchical RCU implementation.
[    0.000000] rcu: 	RCU event tracing is enabled.
[    0.000000] rcu: 	RCU lockdep checking is enabled.
[    0.000000] rcu: 	RCU restricting CPUs from NR_CPUS=4 to nr_cpu_ids=2.
[    0.000000] 	Tracing variant of Tasks RCU enabled.
[    0.000000] rcu: RCU calculated value of scheduler-enlistment delay is 30 jiffies.
[    0.000000] rcu: Adjusting geometry for rcu_fanout_leaf=16, nr_cpu_ids=2
[    0.000000] Running RCU synchronous self tests
[    0.000000] NR_IRQS: 16, nr_irqs: 16, preallocated irqs: 16
[    0.000000] L2C-310 erratum 769419 enabled
[    0.000000] L2C-310 enabling early BRESP for Cortex-A9
[    0.000000] L2C-310 full line of zeros enabled for Cortex-A9
[    0.000000] L2C-310 ID prefetch enabled, offset 16 lines
[    0.000000] L2C-310 dynamic clock gating enabled, standby mode enabled
[    0.000000] L2C-310 cache controller enabled, 16 ways, 512 kB
[    0.000000] L2C-310: CACHE_ID 0x410000c8, AUX_CTRL 0x76450001
[    0.000000] rcu: srcu_init: Setting srcu_struct sizes based on contention.
[    0.000000] Switching to timer-based delay loop, resolution 333ns
[    0.000001] sched_clock: 32 bits at 3000kHz, resolution 333ns, wraps every 715827882841ns
[    0.000029] clocksource: mxc_timer1: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 637086815595 ns
[    0.002351] Console: colour dummy device 80x30
[    0.002580] printk: legacy console [tty0] enabled
[    0.008594] Lock dependency validator: Copyright (c) 2006 Red Hat, Inc., Ingo Molnar
[    0.008765] ... MAX_LOCKDEP_SUBCLASSES:  8
[    0.008861] ... MAX_LOCK_DEPTH:          48
[    0.008956] ... MAX_LOCKDEP_KEYS:        8192
[    0.009052] ... CLASSHASH_SIZE:          4096
[    0.009147] ... MAX_LOCKDEP_ENTRIES:     32768
[    0.009245] ... MAX_LOCKDEP_CHAINS:      65536
[    0.009340] ... CHAINHASH_SIZE:          32768
[    0.009480]  memory used by lock dependency info: 4125 kB
[    0.009595]  memory used for stack traces: 2112 kB
[    0.009698]  per task-struct memory footprint: 1536 bytes
[    0.009867] Calibrating delay loop (skipped), value calculated using timer frequency.. 6.25 BogoMIPS (lpj=10000)
[    0.010080] CPU: Testing write buffer coherency: ok
[    0.010371] CPU0: Spectre v2: using BPIALL workaround
[    0.010486] pid_max: default: 32768 minimum: 301
[    0.011378] Mount-cache hash table entries: 2048 (order: 1, 8192 bytes, linear)
[    0.011559] Mountpoint-cache hash table entries: 2048 (order: 1, 8192 bytes, linear)
[    0.016511] Running RCU synchronous self tests
[    0.016658] Running RCU synchronous self tests
[    0.018144] CPU0: thread -1, cpu 0, socket 0, mpidr 80000000
[    0.023488] RCU Tasks Trace: Setting shift to 1 and lim to 1 rcu_task_cb_adjust=1.
[    0.024160] Running RCU Tasks Trace wait API self tests
[    0.024913] Setting up static identity map for 0x10100000 - 0x10100078
[    0.025563] Callback from call_rcu_tasks_trace() invoked.
[    0.026384] rcu: Hierarchical SRCU implementation.
[    0.026516] rcu: 	Max phase no-delay instances is 1000.
[    0.032236] smp: Bringing up secondary CPUs ...
[    0.032845] smp: Brought up 1 node, 1 CPU
[    0.032975] SMP: Total of 1 processors activated (6.25 BogoMIPS).
[    0.033121] CPU: All CPU(s) started in SVC mode.
[    0.036937] devtmpfs: initialized
[    0.087299] VFP support v0.3: implementor 41 architecture 3 part 30 variant 9 rev 4
[    0.089052] Running RCU synchronous self tests
[    0.089270] Running RCU synchronous self tests
[    0.090258] clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 6370867519511994 ns
[    0.090542] futex hash table entries: 512 (order: 3, 32768 bytes, linear)
[    0.096612] pinctrl core: initialized pinctrl subsystem
[    0.105520] NET: Registered PF_NETLINK/PF_ROUTE protocol family
[    0.114508] DMA: preallocated 256 KiB pool for atomic coherent allocations
[    0.122587] thermal_sys: Registered thermal governor 'step_wise'
[    0.123192] cpuidle: using governor menu
[    0.123845] CPU identified as i.MX6DL, silicon rev 1.2
[    0.147185] platform soc: Fixed dependency cycle(s) with /soc/bus@2000000/gpc@20dc000
[    0.151285] platform 120000.hdmi: Fixed dependency cycle(s) with /soc/ipu@2400000
[    0.185659] platform 20dc000.gpc: Fixed dependency cycle(s) with /soc/bus@2000000/clock-controller@20c4000
[    0.188897] platform 20e0000.iomuxc-gpr:ipu1_csi0_mux: Fixed dependency cycle(s) with /soc/ipu@2400000
[    0.190013] platform 20e0000.iomuxc-gpr:ipu1_csi1_mux: Fixed dependency cycle(s) with /soc/ipu@2400000
[    0.214200] platform 20e0000.iomuxc-gpr:ipu1_csi1_mux: Fixed dependency cycle(s) with /soc/ipu@2400000
[    0.214840] platform 20e0000.iomuxc-gpr:ipu1_csi0_mux: Fixed dependency cycle(s) with /soc/ipu@2400000
[    0.215406] platform 120000.hdmi: Fixed dependency cycle(s) with /soc/ipu@2400000
[    0.215931] platform 2400000.ipu: Fixed dependency cycle(s) with /soc/hdmi@120000
[    0.216543] platform 2400000.ipu: Fixed dependency cycle(s) with /soc/bus@2000000/iomuxc-gpr@20e0000/ipu1_csi1_mux
[    0.217162] platform 2400000.ipu: Fixed dependency cycle(s) with /soc/bus@2000000/iomuxc-gpr@20e0000/ipu1_csi0_mux
[    0.243155] No ATAGs?
[    0.243763] hw-breakpoint: found 5 (+1 reserved) breakpoint and 1 watchpoint registers.
[    0.244037] hw-breakpoint: maximum watchpoint size is 4 bytes.
[    0.251280] imx6dl-pinctrl 20e0000.pinctrl: initialized IMX pinctrl driver
[    0.295820] gpio gpiochip0: Static allocation of GPIO base is deprecated, use dynamic allocation.
[    0.316285] gpio gpiochip1: Static allocation of GPIO base is deprecated, use dynamic allocation.
[    0.332325] gpio gpiochip2: Static allocation of GPIO base is deprecated, use dynamic allocation.
[    0.348682] gpio gpiochip3: Static allocation of GPIO base is deprecated, use dynamic allocation.
[    0.368494] gpio gpiochip4: Static allocation of GPIO base is deprecated, use dynamic allocation.
[    0.385986] gpio gpiochip5: Static allocation of GPIO base is deprecated, use dynamic allocation.
[    0.402566] gpio gpiochip6: Static allocation of GPIO base is deprecated, use dynamic allocation.
[    0.436700] SCSI subsystem initialized
[    0.446725] libata version 3.00 loaded.
[    0.448156] usbcore: registered new interface driver usbfs
[    0.448569] usbcore: registered new interface driver hub
[    0.448932] usbcore: registered new device driver usb
[    0.449688] usb_phy_generic usbphynop1: dummy supplies not allowed for exclusive requests
[    0.450435] usb_phy_generic usbphynop2: dummy supplies not allowed for exclusive requests
[    0.458453] i2c i2c-0: IMX I2C adapter registered
[    0.462486] i2c i2c-1: IMX I2C adapter registered
[    0.465077] i2c i2c-3: IMX I2C adapter registered
[    0.465610] mc: Linux media interface: v0.10
[    0.466030] videodev: Linux video capture interface: v2.00
[    0.467177] pps_core: LinuxPPS API ver. 1 registered
[    0.467320] pps_core: Software ver. 5.3.6 - Copyright 2005-2007 Rodolfo Giometti <giometti@linux.it>
[    0.467582] PTP clock support registered
[    0.484447] Advanced Linux Sound Architecture Driver Initialized.
[    0.491620] Bluetooth: Core ver 2.22
[    0.491918] NET: Registered PF_BLUETOOTH protocol family
[    0.492058] Bluetooth: HCI device and connection manager initialized
[    0.492343] Bluetooth: HCI socket layer initialized
[    0.492515] Bluetooth: L2CAP socket layer initialized
[    0.492912] Bluetooth: SCO socket layer initialized
[    0.503480] mctp: management component transport protocol core
[    0.503644] NET: Registered PF_MCTP protocol family
[    0.505974] vgaarb: loaded
[    0.512217] clocksource: Switched to clocksource mxc_timer1
[    0.516013] VFS: Disk quotas dquot_6.6.0
[    0.516276] VFS: Dquot-cache hash table entries: 1024 (order 0, 4096 bytes)
[    0.623455] NET: Registered PF_INET protocol family
[    0.624147] IP idents hash table entries: 16384 (order: 5, 131072 bytes, linear)
[    0.628392] tcp_listen_portaddr_hash hash table entries: 512 (order: 2, 20480 bytes, linear)
[    0.628790] Table-perturb hash table entries: 65536 (order: 6, 262144 bytes, linear)
[    0.629001] TCP established hash table entries: 8192 (order: 3, 32768 bytes, linear)
[    0.629528] TCP bind hash table entries: 8192 (order: 7, 655360 bytes, linear)
[    0.631952] TCP: Hash tables configured (established 8192 bind 8192)
[    0.633560] MPTCP token hash table entries: 1024 (order: 3, 49152 bytes, linear)
[    0.634076] UDP hash table entries: 512 (order: 3, 49152 bytes, linear)
[    0.634447] UDP-Lite hash table entries: 512 (order: 3, 49152 bytes, linear)
[    0.635527] NET: Registered PF_UNIX/PF_LOCAL protocol family
[    0.648969] RPC: Registered named UNIX socket transport module.
[    0.649227] RPC: Registered udp transport module.
[    0.649355] RPC: Registered tcp transport module.
[    0.649475] RPC: Registered tcp-with-tls transport module.
[    0.649608] RPC: Registered tcp NFSv4.1 backchannel transport module.
[    0.661857] NET: Registered PF_XDP protocol family
[    0.662110] PCI: CLS 0 bytes, default 64
[    0.664094] armv7-pmu pmu: hw perfevents: no interrupt-affinity property, guessing.
[    0.665040] hw perfevents: enabled with armv7_cortex_a9 PMU driver, 7 counters available
[    0.673935] Initialise system trusted keyrings
[    0.687071] workingset: timestamp_bits=30 max_order=18 bucket_order=0
[    0.700363] NFS: Registering the id_resolver key type
[    0.700729] Key type id_resolver registered
[    0.700940] Key type id_legacy registered
[    0.701137] nfs4filelayout_init: NFSv4 File Layout Driver Registering...
[    0.701567] nfs4flexfilelayout_init: NFSv4 Flexfile Layout Driver Registering...
[    0.704185] jffs2: version 2.2. (NAND) © 2001-2006 Red Hat, Inc.
[    0.704695] fuse: init (API version 7.39)
[    0.918213] Key type asymmetric registered
[    0.918475] Asymmetric key parser 'x509' registered
[    0.919067] io scheduler mq-deadline registered
[    0.919206] io scheduler kyber registered
[    0.919379] io scheduler bfq registered
[    1.056373] mxs-dma 110000.dma-controller: initialized
[    1.074874] imx-sdma 20ec000.dma-controller: Direct firmware load for imx/sdma/sdma-imx6q.bin failed with error -2
[    1.075236] imx-sdma 20ec000.dma-controller: Falling back to sysfs fallback for: imx/sdma/sdma-imx6q.bin
[    1.105792] 2020000.serial: ttymxc0 at MMIO 0x2020000 (irq = 269, base_baud = 5000000) is a IMX
[    1.113425] 21e8000.serial: ttymxc1 at MMIO 0x21e8000 (irq = 270, base_baud = 5000000) is a IMX
[    1.114363] printk: legacy console [ttymxc1] enabled
[    2.398982] pfuze100-regulator 0-0008: Full layer: 1, Metal layer: 1
[    2.457395] pfuze100-regulator 0-0008: FAB: 0, FIN: 0
[    2.462706] pfuze100-regulator 0-0008: pfuze100 found.
[    2.483042] 21ec000.serial: ttymxc2 at MMIO 0x21ec000 (irq = 271, base_baud = 5000000) is a IMX
[    2.506810] 21f0000.serial: ttymxc3 at MMIO 0x21f0000 (irq = 272, base_baud = 5000000) is a IMX
[    2.520829] 21f4000.serial: ttymxc4 at MMIO 0x21f4000 (irq = 273, base_baud = 5000000) is a IMX
[    2.551622] dwhdmi-imx 120000.hdmi: Detected HDMI TX controller v1.31a with HDCP (DWC HDMI 3D TX PHY)
[    2.626671] etnaviv etnaviv: bound 130000.gpu (ops gpu_ops)
[    2.633291] etnaviv etnaviv: bound 134000.gpu (ops gpu_ops)
[    2.654350] etnaviv-gpu 130000.gpu: model: GC880, revision: 5106
[    2.662436] etnaviv-gpu 134000.gpu: model: GC320, revision: 5007
[    2.684113] [drm] Initialized etnaviv 1.4.0 20151214 for etnaviv on minor 0
[    2.699854] stackdepot: allocating hash table of 65536 entries via kvcalloc
[    2.735905] imx-drm display-subsystem: bound imx-ipuv3-crtc.2 (ops ipu_crtc_ops)
[    2.744364] imx-drm display-subsystem: bound imx-ipuv3-crtc.3 (ops ipu_crtc_ops)
[    2.753392] imx-drm display-subsystem: bound 120000.hdmi (ops dw_hdmi_imx_ops)
[    2.788282] [drm] Initialized imx-drm 1.0.0 20120507 for display-subsystem on minor 1
[    2.798223] imx-drm display-subsystem: [drm] Cannot find any crtc or sizes
[    2.806989] imx-ipuv3 2400000.ipu: IPUv3H probed
[    2.815941] imx-drm display-subsystem: [drm] Cannot find any crtc or sizes
[    2.950268] brd: module loaded
[    3.033451] loop: module loaded
[    3.323595] zram: Added device: zram0
[    3.359164] CAN device driver interface
[    3.367052] usbcore: registered new interface driver carl9170
[    3.373190] usbcore: registered new device driver r8152-cfgselector
[    3.379872] usbcore: registered new interface driver r8152
[    3.385720] usbcore: registered new interface driver lan78xx
[    3.391743] usbcore: registered new interface driver asix
[    3.397500] usbcore: registered new interface driver ax88179_178a
[    3.403960] usbcore: registered new interface driver cdc_ether
[    3.410156] usbcore: registered new interface driver smsc95xx
[    3.416289] usbcore: registered new interface driver net1080
[    3.422372] usbcore: registered new interface driver cdc_subset
[    3.428749] usbcore: registered new interface driver zaurus
[    3.434696] usbcore: registered new interface driver MOSCHIP usb-ethernet driver
[    3.442529] usbcore: registered new interface driver cdc_ncm
[    3.448556] usbcore: registered new interface driver r8153_ecm
[    3.455068] usbcore: registered new interface driver usb-storage
[    3.542394] ci_hdrc ci_hdrc.1: EHCI Host Controller
[    3.548032] ci_hdrc ci_hdrc.1: new USB bus registered, assigned bus number 1
[    3.584140] ci_hdrc ci_hdrc.1: USB 2.0 started, EHCI 1.00
[    3.601957] usb usb1: New USB device found, idVendor=1d6b, idProduct=0002, bcdDevice= 6.08
[    3.610770] usb usb1: New USB device strings: Mfr=3, Product=2, SerialNumber=1
[    3.618256] usb usb1: Product: EHCI Host Controller
[    3.623347] usb usb1: Manufacturer: Linux 6.8.6-riotboard ehci_hcd
[    3.629763] usb usb1: SerialNumber: ci_hdrc.1
[    3.669864] hub 1-0:1.0: USB hub found
[    3.674372] hub 1-0:1.0: 1 port detected
[    3.703529] SPI driver ads7846 has no spi_device_id for ti,tsc2046
[    3.709989] SPI driver ads7846 has no spi_device_id for ti,ads7843
[    3.716734] SPI driver ads7846 has no spi_device_id for ti,ads7845
[    3.723161] SPI driver ads7846 has no spi_device_id for ti,ads7873
[    3.772270] snvs_rtc 20cc000.snvs:snvs-rtc-lp: registered as rtc0
[    3.778890] snvs_rtc 20cc000.snvs:snvs-rtc-lp: setting system clock to 1970-01-01T00:00:00 UTC (0)
[    3.788907] i2c_dev: i2c /dev entries driver
[    3.848478] device-mapper: ioctl: 4.48.0-ioctl (2023-03-01) initialised: dm-devel@redhat.com
[    3.857613] Bluetooth: HCI UART driver ver 2.3
[    3.862281] Bluetooth: HCI UART protocol H4 registered
[    3.867628] Bluetooth: HCI UART protocol BCSP registered
[    3.873291] Bluetooth: HCI UART protocol LL registered
[    3.878847] Bluetooth: HCI UART protocol Three-wire (H5) registered
[    3.885956] Bluetooth: HCI UART protocol Broadcom registered
[    3.895986] sdhci: Secure Digital Host Controller Interface driver
[    3.902420] sdhci: Copyright(c) Pierre Ossman
[    3.907498] sdhci-pltfm: SDHCI platform and OF driver helper
[    3.923376] caam 2100000.crypto: Entropy delay = 3200
[    3.941256] caam 2100000.crypto: Instantiated RNG4 SH0
[    3.953734] caam 2100000.crypto: Instantiated RNG4 SH1
[    3.959097] caam 2100000.crypto: device ID = 0x0a16010000000100 (Era 4)
[    3.965944] caam 2100000.crypto: job rings = 2, qi = 0
[    4.000414] sdhci-esdhc-imx 2194000.mmc: Got CD GPIO
[    4.005903] sdhci-esdhc-imx 2194000.mmc: Got WP GPIO
[    4.027616] sdhci-esdhc-imx 2198000.mmc: Got CD GPIO
[    4.033652] sdhci-esdhc-imx 2198000.mmc: Got WP GPIO
[    4.057417] usb 1-1: new high-speed USB device number 2 using ci_hdrc
[    4.082161] caam algorithms registered in /proc/crypto
[    4.088015] caam 2100000.crypto: registering rng-caam
[    4.113979] caam 2100000.crypto: rng crypto API alg registered prng-caam
[    4.130567] mmc1: SDHCI controller on 2194000.mmc [2194000.mmc] using ADMA
[    4.139564] mmc3: SDHCI controller on 219c000.mmc [219c000.mmc] using ADMA
[    4.148251] random: crng init done
[    4.158420] mmc2: SDHCI controller on 2198000.mmc [2198000.mmc] using ADMA
[    4.175735] usbcore: registered new interface driver usbhid
[    4.181565] usbhid: USB HID core driver
[    4.240805] sgtl5000 0-000a: sgtl5000 revision 0x11
[    4.251470] usb 1-1: New USB device found, idVendor=1a40, idProduct=0101, bcdDevice= 1.00
[    4.260254] usb 1-1: New USB device strings: Mfr=0, Product=1, SerialNumber=0
[    4.267701] usb 1-1: Product: USB 2.0 Hub [MTT]
[    4.287076] hub 1-1:1.0: USB hub found
[    4.292486] mmc2: new high speed SDHC card at address 0001
[    4.308815] mmcblk2: mmc2:0001 00000 29.3 GiB
[    4.316272] hub 1-1:1.0: 4 ports detected
[    4.338340] sgtl5000 0-000a: Using internal LDO instead of VDDD: check ER1 erratum
[    4.354613] mmc3: new DDR MMC card at address 0001
[    4.379312] mmcblk3: mmc3:0001 MMC04G 3.58 GiB
[    4.386832]  mmcblk2: p1 p2
[    4.424532]  mmcblk3: p1 p2 p3 < p5 p6 p7 p8 > p4
[    4.443100] mmcblk3: p4 size 5324800 extends beyond EOD, truncated
[    4.472694] mmcblk3boot0: mmc3:0001 MMC04G 2.00 MiB
[    4.505983] mmcblk3boot1: mmc3:0001 MMC04G 2.00 MiB
[    4.544500] mmcblk3rpmb: mmc3:0001 MMC04G 128 KiB, chardev (243:0)
[    4.570677] fsl-ssi-dai 2028000.ssi: No cache defaults, reading back from HW
[    4.617157] imx-sgtl5000 sound: ASoC: driver name too long 'imx6-riotboard-sgtl5000' -> 'imx6-riotboard-'
[    4.649052] ipip: IPv4 and MPLS over IPv4 tunneling driver
[    4.657934] Initializing XFRM netlink socket
[    4.663043] NET: Registered PF_INET6 protocol family
[    4.683524] Segment Routing with IPv6
[    4.688051] RPL Segment Routing with IPv6
[    4.692408] In-situ OAM (IOAM) with IPv6
[    4.696732] sit: IPv6, IPv4 and MPLS over IPv4 tunneling driver
[    4.706279] NET: Registered PF_PACKET protocol family
[    4.711586] can: controller area network core
[    4.716353] NET: Registered PF_CAN protocol family
[    4.721370] can: raw protocol
[    4.724721] can: broadcast manager protocol
[    4.729166] can: netlink gateway - max_hops=1
[    4.734986] NET: Registered PF_KCM protocol family
[    4.740887] Key type dns_resolver registered
[    4.758950] Registering SWP/SWPB emulation handler
[    4.830084] usb 1-1.4: new high-speed USB device number 3 using ci_hdrc
[    4.843573] Loading compiled-in X.509 certificates
[    5.016730] pps pps0: new PPS source ptp0
[    5.025931] fec 2188000.ethernet: Invalid MAC address: 00:00:00:00:00:00
[    5.032976] fec 2188000.ethernet: Using random MAC address: 66:78:d8:ab:97:69
[    5.082318] fec 2188000.ethernet eth0: registered PHC device 0
[    5.116233] imx_thermal 20c8000.anatop:tempmon: Commercial CPU temperature grade - max:95C critical:90C passive:85C
[    5.132992] usb 1-1.4: New USB device found, idVendor=0bda, idProduct=c811, bcdDevice= 2.00
[    5.142521] usb 1-1.4: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[    5.150580] usb 1-1.4: Product: 802.11ac NIC
[    5.155528] usb 1-1.4: Manufacturer: Realtek
[    5.160200] usb 1-1.4: SerialNumber: 123456
[    5.177922] psci_checker: Missing PSCI operations, aborting tests
[    5.184325] of_cfs_init
[    5.187438] of_cfs_init: OK
[    5.190923] cfg80211: Loading compiled-in X.509 certificates for regulatory database
[    5.205113] Loaded X.509 cert 'sforshee: 00b28ddf47aef9cea7'
[    5.213851] Loaded X.509 cert 'wens: 61c038651aabdcf94bd0ac7ff06c7248db18c600'
[    5.222193] clk: Disabling unused clocks
[    5.227285] ALSA device list:
[    5.230433]   #0: imx6-riotboard-sgtl5000
[    5.236808] platform regulatory.0: Direct firmware load for regulatory.db failed with error -2
[    5.245741] platform regulatory.0: Falling back to sysfs fallback for: regulatory.db
[    5.285729] EXT4-fs (mmcblk2p2): INFO: recovery required on readonly filesystem
[    5.293942] EXT4-fs (mmcblk2p2): write access will be enabled during recovery
[    5.494867] EXT4-fs (mmcblk2p2): recovery complete
[    5.507552] EXT4-fs (mmcblk2p2): mounted filesystem 4ca0bb68-64e8-45f6-9589-ec30fb4dd792 ro with ordered data mode. Quota mode: none.
[    5.520204] VFS: Mounted root (ext4 filesystem) readonly on device 179:2.
[    5.541255] devtmpfs: mounted
[    5.547916] Freeing unused kernel image (initmem) memory: 1024K
[    5.556189] Run /sbin/init as init process
[    5.560515]   with arguments:
[    5.560543]     /sbin/init
[    5.560568]     ttymxc1,115200
[    5.560590]   with environment:
[    5.560614]     HOME=/
[    5.560637]     TERM=linux
[    5.560660]     fbmem=10M
[    6.334940] systemd[1]: System time before build time, advancing clock.
[    6.456380] systemd[1]: Inserted module 'autofs4'
[    6.586042] systemd[1]: systemd 252.22-1~deb12u1 running in system mode (+PAM +AUDIT +SELINUX +APPARMOR +IMA +SMACK +SECCOMP +GCRYPT -GNUTLS +OPENSSL +ACL +BLKID +CURL +ELFUTILS +FIDO2 +IDN2 -IDN +IPTC +KMOD +LIBCRYPTSETUP +LIBFDISK +PCRE2 -PWQUALITY +P11KIT +QRENCODE +TPM2 +BZIP2 +LZ4 +XZ +ZLIB +ZSTD -BPF_FRAMEWORK -XKBCOMMON +UTMP +SYSVINIT default-hierarchy=unified)
[    6.620326] systemd[1]: Detected architecture arm.
[    8.656093] systemd[1]: /etc/systemd/system/rc-local.service:12: Support for option SysVStartPriority= has been removed and it is ignored
[    9.080639] systemd[1]: Queued start job for default target graphical.target.
[    9.101027] systemd[1]: Created slice system-getty.slice - Slice /system/getty.
[    9.136610] systemd[1]: Created slice system-modprobe.slice - Slice /system/modprobe.
[    9.169945] systemd[1]: Created slice system-serial\x2dgetty.slice - Slice /system/serial-getty.
[    9.203980] systemd[1]: Created slice system-systemd\x2dfsck.slice - Slice /system/systemd-fsck.
[    9.234890] systemd[1]: Created slice user.slice - User and Session Slice.
[    9.264273] systemd[1]: Started systemd-ask-password-console.path - Dispatch Password Requests to Console Directory Watch.
[    9.297122] systemd[1]: Started systemd-ask-password-wall.path - Forward Password Requests to Wall Directory Watch.
[    9.336658] systemd[1]: Set up automount proc-sys-fs-binfmt_misc.automount - Arbitrary Executable File Formats File System Automount Point.
[    9.370091] systemd[1]: Expecting device dev-disk-by\x2duuid-920754b8\x2d7959\x2d4e6b\x2da9c6\x2dcfa721fa3c9a.device - /dev/disk/by-uuid/920754b8-7959-4e6b-a9c6-cfa721fa3c9a...
[    9.405824] systemd[1]: Expecting device dev-ttymxc1.device - /dev/ttymxc1...
[    9.432660] systemd[1]: Reached target cryptsetup.target - Local Encrypted Volumes.
[    9.459541] systemd[1]: Reached target integritysetup.target - Local Integrity Protected Volumes.
[    9.486129] systemd[1]: Reached target network-pre.target - Preparation for Network.
[    9.513377] systemd[1]: Reached target paths.target - Path Units.
[    9.539416] systemd[1]: Reached target remote-fs.target - Remote File Systems.
[    9.566617] systemd[1]: Reached target slices.target - Slice Units.
[    9.592798] systemd[1]: Reached target swap.target - Swaps.
[    9.616823] systemd[1]: Reached target veritysetup.target - Local Verity Protected Volumes.
[    9.648413] systemd[1]: Listening on systemd-fsckd.socket - fsck to fsckd communication Socket.
[    9.677506] systemd[1]: Listening on systemd-initctl.socket - initctl Compatibility Named Pipe.
[    9.741350] systemd[1]: systemd-journald-audit.socket - Journal Audit Socket was skipped because of an unmet condition check (ConditionSecurity=audit).
[    9.758082] systemd[1]: Listening on systemd-journald-dev-log.socket - Journal Socket (/dev/log).
[    9.788035] systemd[1]: Listening on systemd-journald.socket - Journal Socket.
[    9.838439] systemd[1]: Listening on systemd-udevd-control.socket - udev Control Socket.
[    9.867831] systemd[1]: Listening on systemd-udevd-kernel.socket - udev Kernel Socket.
[    9.897989] systemd[1]: dev-hugepages.mount - Huge Pages File System was skipped because of an unmet condition check (ConditionPathExists=/sys/kernel/mm/hugepages).
[    9.937307] systemd[1]: Mounting dev-mqueue.mount - POSIX Message Queue File System...
[    9.996524] systemd[1]: Mounting sys-kernel-debug.mount - Kernel Debug File System...
[   10.069729] systemd[1]: Mounting sys-kernel-tracing.mount - Kernel Trace File System...
[   10.144619] systemd[1]: Starting keyboard-setup.service - Set the console keyboard layout...
[   10.213775] systemd[1]: Starting kmod-static-nodes.service - Create List of Static Device Nodes...
[   10.302995] systemd[1]: Starting modprobe@configfs.service - Load Kernel Module configfs...
[   10.387170] systemd[1]: Starting modprobe@dm_mod.service - Load Kernel Module dm_mod...
[   10.473855] systemd[1]: Starting modprobe@drm.service - Load Kernel Module drm...
[   10.557228] systemd[1]: Starting modprobe@efi_pstore.service - Load Kernel Module efi_pstore...
[   10.644914] systemd[1]: Starting modprobe@fuse.service - Load Kernel Module fuse...
[   10.743854] systemd[1]: Starting modprobe@loop.service - Load Kernel Module loop...
[   10.843783] systemd[1]: Starting systemd-fsck-root.service - File System Check on Root Device...
[   10.894466] systemd[1]: systemd-journald.service: unit configures an IP firewall, but the local system does not support BPF/cgroup firewalling.
[   10.932460] systemd[1]: (This warning is only shown for the first unit using IP firewalling.)
[   10.997227] systemd[1]: Starting systemd-journald.service - Journal Service...
[   11.100383] systemd[1]: Starting systemd-modules-load.service - Load Kernel Modules...
[   11.227176] systemd[1]: Starting systemd-udev-trigger.service - Coldplug All udev Devices...
[   11.478489] systemd[1]: Mounted dev-mqueue.mount - POSIX Message Queue File System.
[   11.545143] systemd[1]: Mounted sys-kernel-debug.mount - Kernel Debug File System.
[   11.594631] systemd[1]: Mounted sys-kernel-tracing.mount - Kernel Trace File System.
[   11.709889] systemd[1]: Finished kmod-static-nodes.service - Create List of Static Device Nodes.
[   11.930844] systemd[1]: modprobe@configfs.service: Deactivated successfully.
[   11.983648] systemd[1]: Finished modprobe@configfs.service - Load Kernel Module configfs.
[   12.061547] systemd[1]: modprobe@dm_mod.service: Deactivated successfully.
[   12.112499] systemd[1]: Finished modprobe@dm_mod.service - Load Kernel Module dm_mod.
[   12.172572] systemd[1]: modprobe@drm.service: Deactivated successfully.
[   12.209491] systemd[1]: Finished modprobe@drm.service - Load Kernel Module drm.
[   12.271585] systemd[1]: modprobe@efi_pstore.service: Deactivated successfully.
[   12.309176] systemd[1]: Finished modprobe@efi_pstore.service - Load Kernel Module efi_pstore.
[   12.382731] systemd[1]: modprobe@fuse.service: Deactivated successfully.
[   12.423914] systemd[1]: Finished modprobe@fuse.service - Load Kernel Module fuse.
[   12.497494] systemd[1]: modprobe@loop.service: Deactivated successfully.
[   12.539475] systemd[1]: Finished modprobe@loop.service - Load Kernel Module loop.
[   12.602919] systemd[1]: Finished systemd-fsck-root.service - File System Check on Root Device.
[   12.673195] systemd[1]: Finished systemd-modules-load.service - Load Kernel Modules.
[   12.789726] systemd[1]: Mounting sys-fs-fuse-connections.mount - FUSE Control File System...
[   12.913064] systemd[1]: Mounting sys-kernel-config.mount - Kernel Configuration File System...
[   13.037013] systemd[1]: Started systemd-fsckd.service - File System Check Daemon to report status.
[   13.166910] systemd[1]: Starting systemd-remount-fs.service - Remount Root and Kernel File Systems...
[   13.243427] systemd[1]: systemd-repart.service - Repartition Root Disk was skipped because no trigger condition checks were met.
[   13.396933] systemd[1]: Starting systemd-sysctl.service - Apply Kernel Variables...
[   13.619826] systemd[1]: Started systemd-journald.service - Journal Service.
[   16.208850] EXT4-fs (mmcblk2p2): re-mounted 4ca0bb68-64e8-45f6-9589-ec30fb4dd792 r/w. Quota mode: none.
[   16.803146] systemd-journald[142]: Received client request to flush runtime journal.
[   16.933274] systemd-journald[142]: File /var/log/journal/9afc59e34c0f493aa6f6c894265ec651/system.journal corrupted or uncleanly shut down, renaming and replacing.
[   24.494994] imx_media_common: module is from the staging directory, the quality is unknown, you have been warned.
[   24.591357] cfg80211: failed to load regulatory.db
[   24.664385] imx6_media: module is from the staging directory, the quality is unknown, you have been warned.
[   25.129802] coda 2040000.vpu: Direct firmware load for vpu_fw_imx6d.bin failed with error -2
[   25.138573] coda 2040000.vpu: Falling back to sysfs fallback for: vpu_fw_imx6d.bin
[   25.828124] imx-sdma 20ec000.dma-controller: external firmware not found, using ROM firmware
[   26.262910] imx6_media_csi: module is from the staging directory, the quality is unknown, you have been warned.
[   26.442939] imx-ipuv3-csi imx-ipuv3-csi.0: Registered ipu1_csi0 capture as /dev/video0
[   26.509330] imx-ipuv3 2400000.ipu: Registered ipu1_ic_prpenc capture as /dev/video1
[   26.582542] imx-ipuv3 2400000.ipu: Registered ipu1_ic_prpvf capture as /dev/video2
[   26.645697] imx-ipuv3-csi imx-ipuv3-csi.1: Registered ipu1_csi1 capture as /dev/video3
[   26.813841] imx-media: Registered ipu_ic_pp csc/scaler as /dev/video4
[   27.284188] rtw_8821cu 1-1.4:1.0: Firmware version 24.11.0, H2C version 12
[   28.095952] coda 2040000.vpu: Using fallback firmware vpu/vpu_fw_imx6d.bin
[   28.194946] coda 2040000.vpu: Firmware code revision: 46072
[   28.200896] coda 2040000.vpu: Initialized CODA960.
[   28.205935] coda 2040000.vpu: Firmware version: 3.1.1
[   28.293242] coda 2040000.vpu: coda-jpeg-encoder registered as video5
[   28.362455] coda 2040000.vpu: coda-jpeg-decoder registered as video6
[   28.436491] coda 2040000.vpu: coda-video-encoder registered as video7
[   28.509275] coda 2040000.vpu: coda-video-decoder registered as video8
[   29.974931] usbcore: registered new interface driver rtw_8821cu
[   35.855726] EXT4-fs (mmcblk2p1): mounted filesystem 920754b8-7959-4e6b-a9c6-cfa721fa3c9a r/w with ordered data mode. Quota mode: none.
[   37.889837] Qualcomm Atheros AR8035 2188000.ethernet-1:04: attached PHY driver (mii_bus:phy_addr=2188000.ethernet-1:04, irq=56)
[   39.464349] systemd-journald[142]: Oldest entry in /var/log/journal/9afc59e34c0f493aa6f6c894265ec651/system.journal is older than the configured file retention duration (1month), suggesting rotation.
[   39.513511] systemd-journald[142]: /var/log/journal/9afc59e34c0f493aa6f6c894265ec651/system.journal: Journal header limits reached or header out-of-date, rotating.
[   40.677086] fec 2188000.ethernet eth0: Link is Up - 1Gbps/Full - flow control rx/tx
[   41.100937] systemd-journald[142]: Failed to read journal file /var/log/journal/9afc59e34c0f493aa6f6c894265ec651/user-1000.journal for rotation, trying to move it out of the way: Device or resource busy
[   44.657705] 8021q: 802.1Q VLAN Support v1.8
[   47.661243] tun: Universal TUN/TAP device driver, 1.6
```

## Issues

* There will be a deadlock when loading the 88XXau module, but it does not affect the use. I have tested it and confirmed that it is a problem with 88XXau.



## Credits

I would like to thank the Kail-ARM project for inspiring me.
