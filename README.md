# RIotBoard 
## Product Introduction
The RIoTboard is an evaluation platform featuring the powerful i.MX 6Solo, a
multimedia application processor with ARM Cortex-A9 core at 1 GHz from Freescale
Semiconductor. The platform helps evaluate the rich set of peripherals and includes
a 10/100/Gb Ethernet port, HDMI v1.4, LVDS, analog headphone/microphone, uSD
and SD card interface, USB, serial port, JTAG, 2 camera interfaces, GPIO boot
configuration interface, and expansion port.

Releases:
* [u-boot - 2018.05 ](#u-boot)
* Debian 11(bullseye)

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

## Kernel 6.5.7

Testing mainline **6.5.7**, current status:

* Eth0 - ok
* Wifi Realtek 8812AU/8821AU (5 GHz) - ok
* Wifi mt7601u (2.4 GHz) - ok
* eMMC - ok
* hdmi - ok (for debugging)
* sound - JACK output and HDMI output (ok)
* USB2 - ok

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
## Kernel 6.5.7

Kernel 6.5.7 is rock solid in RIotBoard, building **Kernel 6.5.7** with gcc-12 on uSD for testing new devfreq and checking stability.
The board works great in real time using Gstreamer and a hardware h264 encoder.

![webrtc](https://github.com/yjdwbj/imx6-riotboard/raw/master/webrtc-sendonly-htop.png)


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

## Boot log (booting from uSD Card)

```sh
[    0.000000] Booting Linux on physical CPU 0x0
[    0.000000] Linux version 6.5.7-lcy-20231018 (yjdwbj@gmail.com) (arm-linux-gnueabi-gcc (Debian 12.2.0-14) 12.2.0, GNU ld (GNU Binutils for Debian) 2.40) #1 SMP Wed Oct 18 11:36:59 CST 2023
[    0.000000] CPU: ARMv7 Processor [412fc09a] revision 10 (ARMv7), cr=10c5387d
[    0.000000] CPU: PIPT / VIPT nonaliasing data cache, VIPT aliasing instruction cache
[    0.000000] OF: fdt: Machine model: RIoTboard i.MX6S
[    0.000000] Memory policy: Data cache writeback
[    0.000000] cma: Reserved 128 MiB at 0x48000000
[    0.000000] Zone ranges:
[    0.000000]   Normal   [mem 0x0000000010000000-0x0000000035ffffff]
[    0.000000]   HighMem  [mem 0x0000000036000000-0x000000004fffffff]
[    0.000000] Movable zone start for each node
[    0.000000] Early memory node ranges
[    0.000000]   node   0: [mem 0x0000000010000000-0x000000004fffffff]
[    0.000000] Initmem setup node 0 [mem 0x0000000010000000-0x000000004fffffff]
[    0.000000] CPU: All CPU(s) started in SVC mode.
[    0.000000] percpu: Embedded 18 pages/cpu s43572 r8192 d21964 u73728
[    0.000000] pcpu-alloc: s43572 r8192 d21964 u73728 alloc=18*4096
[    0.000000] pcpu-alloc: [0] 0 [0] 1 
[    0.000000] Kernel command line: ttymxc1,115200 nosmp video=mxcfb0:dev=hdmi,1280x720M@60,bpp=32 net.ifnames=0 video=mxcfb1:off fbmem=10M vmalloc=400M rootwait root=PARTUUID=de3dc445-02 init=/sbin/init
[    0.000000] Unknown kernel command line parameters "fbmem=10M", will be passed to user space.
[    0.000000] Dentry cache hash table entries: 131072 (order: 7, 524288 bytes, linear)
[    0.000000] Inode-cache hash table entries: 65536 (order: 6, 262144 bytes, linear)
[    0.000000] Built 1 zonelists, mobility grouping on.  Total pages: 260928
[    0.000000] mem auto-init: stack:all(zero), heap alloc:off, heap free:off
[    0.000000] Memory: 873460K/1048576K available (17408K kernel code, 2374K rwdata, 6168K rodata, 1024K init, 6750K bss, 44044K reserved, 131072K cma-reserved, 294912K highmem)
[    0.000000] SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=2, Nodes=1
[    0.000000] trace event string verifier disabled
[    0.000000] Running RCU self tests
[    0.000000] Running RCU synchronous self tests
[    0.000000] rcu: Hierarchical RCU implementation.
[    0.000000] rcu: 	RCU event tracing is enabled.
[    0.000000] rcu: 	RCU lockdep checking is enabled.
[    0.000000] rcu: 	RCU restricting CPUs from NR_CPUS=4 to nr_cpu_ids=2.
[    0.000000] 	Tracing variant of Tasks RCU enabled.
[    0.000000] rcu: RCU calculated value of scheduler-enlistment delay is 10 jiffies.
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
[    0.002172] Console: colour dummy device 80x30
[    0.002364] printk: console [tty0] enabled
[    0.007551] Lock dependency validator: Copyright (c) 2006 Red Hat, Inc., Ingo Molnar
[    0.007710] ... MAX_LOCKDEP_SUBCLASSES:  8
[    0.007801] ... MAX_LOCK_DEPTH:          48
[    0.007888] ... MAX_LOCKDEP_KEYS:        8192
[    0.007976] ... CLASSHASH_SIZE:          4096
[    0.008064] ... MAX_LOCKDEP_ENTRIES:     32768
[    0.008154] ... MAX_LOCKDEP_CHAINS:      65536
[    0.008242] ... CHAINHASH_SIZE:          32768
[    0.008333]  memory used by lock dependency info: 4125 kB
[    0.008436]  memory used for stack traces: 2112 kB
[    0.008530]  per task-struct memory footprint: 1536 bytes
[    0.008747] Calibrating delay loop (skipped), value calculated using timer frequency.. 6.00 BogoMIPS (lpj=30000)
[    0.008956] CPU: Testing write buffer coherency: ok
[    0.009241] CPU0: Spectre v2: using BPIALL workaround
[    0.009348] pid_max: default: 32768 minimum: 301
[    0.010539] Mount-cache hash table entries: 2048 (order: 1, 8192 bytes, linear)
[    0.010714] Mountpoint-cache hash table entries: 2048 (order: 1, 8192 bytes, linear)
[    0.015458] Running RCU synchronous self tests
[    0.015597] Running RCU synchronous self tests
[    0.017230] CPU0: thread -1, cpu 0, socket 0, mpidr 80000000
[    0.022093] RCU Tasks Trace: Setting shift to 1 and lim to 1 rcu_task_cb_adjust=1.
[    0.022682] Running RCU-tasks wait API self tests
[    0.023158] Setting up static identity map for 0x10100000 - 0x10100078
[    0.024092] Callback from call_rcu_tasks_trace() invoked.
[    0.024584] rcu: Hierarchical SRCU implementation.
[    0.024701] rcu: 	Max phase no-delay instances is 1000.
[    0.030123] smp: Bringing up secondary CPUs ...
[    0.030627] smp: Brought up 1 node, 1 CPU
[    0.030743] SMP: Total of 1 processors activated (6.00 BogoMIPS).
[    0.030877] CPU: All CPU(s) started in SVC mode.
[    0.034365] devtmpfs: initialized
[    0.077541] VFP support v0.3: implementor 41 architecture 3 part 30 variant 9 rev 4
[    0.079249] Running RCU synchronous self tests
[    0.079615] Running RCU synchronous self tests
[    0.080478] clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 19112604462750000 ns
[    0.080735] futex hash table entries: 512 (order: 3, 32768 bytes, linear)
[    0.086346] pinctrl core: initialized pinctrl subsystem
[    0.093086] NET: Registered PF_NETLINK/PF_ROUTE protocol family
[    0.101921] DMA: preallocated 256 KiB pool for atomic coherent allocations
[    0.109668] thermal_sys: Registered thermal governor 'step_wise'
[    0.110208] cpuidle: using governor menu
[    0.110825] CPU identified as i.MX6DL, silicon rev 1.2
[    0.132471] platform soc: Fixed dependency cycle(s) with /soc/bus@2000000/gpc@20dc000
[    0.196709] platform 2400000.ipu: Fixed dependency cycle(s) with /soc/hdmi@120000/ports/port@1/endpoint
[    0.197048] platform 2400000.ipu: Fixed dependency cycle(s) with /soc/hdmi@120000/ports/port@0/endpoint
[    0.197346] platform 2400000.ipu: Fixed dependency cycle(s) with /soc/bus@2000000/iomuxc-gpr@20e0000/ipu1_csi1_mux/port@5/endpoint
[    0.197704] platform 2400000.ipu: Fixed dependency cycle(s) with /soc/bus@2000000/iomuxc-gpr@20e0000/ipu1_csi0_mux/port@5/endpoint
[    0.222672] No ATAGs?
[    0.223171] hw-breakpoint: found 5 (+1 reserved) breakpoint and 1 watchpoint registers.
[    0.223432] hw-breakpoint: maximum watchpoint size is 4 bytes.
[    0.230330] imx6dl-pinctrl 20e0000.pinctrl: initialized IMX pinctrl driver
[    1.492072] gpio gpiochip0: Static allocation of GPIO base is deprecated, use dynamic allocation.
[    1.504194] gpio gpiochip1: Static allocation of GPIO base is deprecated, use dynamic allocation.
[    1.514471] gpio gpiochip2: Static allocation of GPIO base is deprecated, use dynamic allocation.
[    1.524542] gpio gpiochip3: Static allocation of GPIO base is deprecated, use dynamic allocation.
[    1.535260] gpio gpiochip4: Static allocation of GPIO base is deprecated, use dynamic allocation.
[    1.545921] gpio gpiochip5: Static allocation of GPIO base is deprecated, use dynamic allocation.
[    1.556756] gpio gpiochip6: Static allocation of GPIO base is deprecated, use dynamic allocation.
[    1.576439] SCSI subsystem initialized
[    1.583592] libata version 3.00 loaded.
[    1.584857] usbcore: registered new interface driver usbfs
[    1.585269] usbcore: registered new interface driver hub
[    1.585576] usbcore: registered new device driver usb
[    1.586194] usb_phy_generic usbphynop1: dummy supplies not allowed for exclusive requests
[    1.586909] usb_phy_generic usbphynop2: dummy supplies not allowed for exclusive requests
[    1.594934] i2c i2c-0: IMX I2C adapter registered
[    1.598753] i2c i2c-1: IMX I2C adapter registered
[    1.601239] i2c i2c-3: IMX I2C adapter registered
[    1.601759] mc: Linux media interface: v0.10
[    1.602259] videodev: Linux video capture interface: v2.00
[    1.603247] pps_core: LinuxPPS API ver. 1 registered
[    1.603374] pps_core: Software ver. 5.3.6 - Copyright 2005-2007 Rodolfo Giometti <giometti@linux.it>
[    1.603615] PTP clock support registered
[    1.605713] Advanced Linux Sound Architecture Driver Initialized.
[    1.612108] Bluetooth: Core ver 2.22
[    1.612388] NET: Registered PF_BLUETOOTH protocol family
[    1.612511] Bluetooth: HCI device and connection manager initialized
[    1.612770] Bluetooth: HCI socket layer initialized
[    1.612914] Bluetooth: L2CAP socket layer initialized
[    1.613201] Bluetooth: SCO socket layer initialized
[    1.620196] vgaarb: loaded
[    1.622590] clocksource: Switched to clocksource mxc_timer1
[    1.625642] VFS: Disk quotas dquot_6.6.0
[    1.625886] VFS: Dquot-cache hash table entries: 1024 (order 0, 4096 bytes)
[    2.203053] NET: Registered PF_INET protocol family
[    2.203824] IP idents hash table entries: 16384 (order: 5, 131072 bytes, linear)
[    2.207937] tcp_listen_portaddr_hash hash table entries: 512 (order: 2, 20480 bytes, linear)
[    2.208220] Table-perturb hash table entries: 65536 (order: 6, 262144 bytes, linear)
[    2.208412] TCP established hash table entries: 8192 (order: 3, 32768 bytes, linear)
[    2.208918] TCP bind hash table entries: 8192 (order: 7, 655360 bytes, linear)
[    2.211332] TCP: Hash tables configured (established 8192 bind 8192)
[    2.212228] UDP hash table entries: 512 (order: 3, 49152 bytes, linear)
[    2.212742] UDP-Lite hash table entries: 512 (order: 3, 49152 bytes, linear)
[    2.213639] NET: Registered PF_UNIX/PF_LOCAL protocol family
[    2.217877] RPC: Registered named UNIX socket transport module.
[    2.218120] RPC: Registered udp transport module.
[    2.218234] RPC: Registered tcp transport module.
[    2.218342] RPC: Registered tcp-with-tls transport module.
[    2.218465] RPC: Registered tcp NFSv4.1 backchannel transport module.
[    2.221438] NET: Registered PF_XDP protocol family
[    2.221602] PCI: CLS 0 bytes, default 64
[    2.223804] armv7-pmu pmu: hw perfevents: no interrupt-affinity property, guessing.
[    2.226768] hw perfevents: enabled with armv7_cortex_a9 PMU driver, 7 counters available
[    2.235338] Initialise system trusted keyrings
[    2.236535] workingset: timestamp_bits=30 max_order=18 bucket_order=0
[    2.240055] NFS: Registering the id_resolver key type
[    2.240360] Key type id_resolver registered
[    2.240554] Key type id_legacy registered
[    2.240761] nfs4filelayout_init: NFSv4 File Layout Driver Registering...
[    2.241007] nfs4flexfilelayout_init: NFSv4 Flexfile Layout Driver Registering...
[    2.243546] ksmbd: The ksmbd server is experimental
[    2.243686] jffs2: version 2.2. (NAND) © 2001-2006 Red Hat, Inc.
[    2.244165] fuse: init (API version 7.38)
[    2.388938] Key type asymmetric registered
[    2.389181] Asymmetric key parser 'x509' registered
[    2.389770] bounce: pool size: 64 pages
[    2.390077] io scheduler mq-deadline registered
[    2.390204] io scheduler kyber registered
[    2.390365] io scheduler bfq registered
[    2.428659] mxs-dma 110000.dma-controller: initialized
[    2.446240] imx-sdma 20ec000.dma-controller: Direct firmware load for imx/sdma/sdma-imx6q.bin failed with error -2
[    2.446577] imx-sdma 20ec000.dma-controller: Falling back to sysfs fallback for: imx/sdma/sdma-imx6q.bin
[    2.455488] 2020000.serial: ttymxc0 at MMIO 0x2020000 (irq = 269, base_baud = 5000000) is a IMX
[    2.463304] 21e8000.serial: ttymxc1 at MMIO 0x21e8000 (irq = 270, base_baud = 5000000) is a IMX
[    2.464120] printk: console [ttymxc1] enabled
[    3.537306] 21ec000.serial: ttymxc2 at MMIO 0x21ec000 (irq = 271, base_baud = 5000000) is a IMX
[    3.551012] 21f0000.serial: ttymxc3 at MMIO 0x21f0000 (irq = 272, base_baud = 5000000) is a IMX
[    3.565055] 21f4000.serial: ttymxc4 at MMIO 0x21f4000 (irq = 273, base_baud = 5000000) is a IMX
[    3.575918] pfuze100-regulator 0-0008: Full layer: 1, Metal layer: 1
[    3.591886] dwhdmi-imx 120000.hdmi: Detected HDMI TX controller v1.31a with HDCP (DWC HDMI 3D TX PHY)
[    3.614187] pfuze100-regulator 0-0008: FAB: 0, FIN: 0
[    3.619409] pfuze100-regulator 0-0008: pfuze100 found.
[    3.645076] etnaviv etnaviv: bound 130000.gpu (ops gpu_ops)
[    3.653726] etnaviv etnaviv: bound 134000.gpu (ops gpu_ops)
[    3.659496] etnaviv-gpu 130000.gpu: model: GC880, revision: 5106
[    3.667340] etnaviv-gpu 134000.gpu: model: GC320, revision: 5007
[    3.679354] [drm] Initialized etnaviv 1.3.0 20151214 for etnaviv on minor 0
[    3.694826] stackdepot: allocating hash table of 65536 entries via kvcalloc
[    3.709867] imx-drm display-subsystem: bound imx-ipuv3-crtc.2 (ops ipu_crtc_ops)
[    3.718338] imx-drm display-subsystem: bound imx-ipuv3-crtc.3 (ops ipu_crtc_ops)
[    3.727242] imx-drm display-subsystem: bound 120000.hdmi (ops dw_hdmi_imx_ops)
[    3.740599] [drm] Initialized imx-drm 1.0.0 20120507 for display-subsystem on minor 1
[    3.750803] imx-drm display-subsystem: [drm] Cannot find any crtc or sizes
[    3.759414] imx-ipuv3 2400000.ipu: IPUv3H probed
[    3.767054] imx-drm display-subsystem: [drm] Cannot find any crtc or sizes
[    3.834559] brd: module loaded
[    3.878508] loop: module loaded
[    3.905123] CAN device driver interface
[    3.912261] usbcore: registered new interface driver carl9170
[    3.918420] usbcore: registered new interface driver mt7601u
[    3.924436] usbcore: registered new interface driver rt2500usb
[    3.930547] usbcore: registered new interface driver rt73usb
[    3.936587] usbcore: registered new interface driver rt2800usb
[    3.942750] usbcore: registered new interface driver rtl8xxxu
[    3.948744] usbcore: registered new device driver r8152-cfgselector
[    3.955369] usbcore: registered new interface driver r8152
[    3.961126] usbcore: registered new interface driver lan78xx
[    3.967177] usbcore: registered new interface driver asix
[    3.972931] usbcore: registered new interface driver ax88179_178a
[    3.979330] usbcore: registered new interface driver cdc_ether
[    3.985515] usbcore: registered new interface driver smsc95xx
[    3.991538] usbcore: registered new interface driver net1080
[    3.997558] usbcore: registered new interface driver cdc_subset
[    4.003829] usbcore: registered new interface driver zaurus
[    4.009681] usbcore: registered new interface driver MOSCHIP usb-ethernet driver
[    4.017491] usbcore: registered new interface driver cdc_ncm
[    4.023529] usbcore: registered new interface driver r8153_ecm
[    4.029948] usbcore: registered new interface driver usb-storage
[    4.060104] ci_hdrc ci_hdrc.1: EHCI Host Controller
[    4.065817] ci_hdrc ci_hdrc.1: new USB bus registered, assigned bus number 1
[    4.102518] ci_hdrc ci_hdrc.1: USB 2.0 started, EHCI 1.00
[    4.110551] usb usb1: New USB device found, idVendor=1d6b, idProduct=0002, bcdDevice= 6.05
[    4.119292] usb usb1: New USB device strings: Mfr=3, Product=2, SerialNumber=1
[    4.126764] usb usb1: Product: EHCI Host Controller
[    4.131773] usb usb1: Manufacturer: Linux 6.5.7-lcy-20231018 ehci_hcd
[    4.138432] usb usb1: SerialNumber: ci_hdrc.1
[    4.148841] hub 1-0:1.0: USB hub found
[    4.153386] hub 1-0:1.0: 1 port detected
[    4.168857] SPI driver ads7846 has no spi_device_id for ti,tsc2046
[    4.175347] SPI driver ads7846 has no spi_device_id for ti,ads7843
[    4.181667] SPI driver ads7846 has no spi_device_id for ti,ads7845
[    4.188383] SPI driver ads7846 has no spi_device_id for ti,ads7873
[    4.210717] snvs_rtc 20cc000.snvs:snvs-rtc-lp: registered as rtc0
[    4.217389] snvs_rtc 20cc000.snvs:snvs-rtc-lp: setting system clock to 2023-10-18T04:23:25 UTC (1697603005)
[    4.228077] i2c_dev: i2c /dev entries driver
[    4.252994] Bluetooth: HCI UART driver ver 2.3
[    4.257587] Bluetooth: HCI UART protocol H4 registered
[    4.263063] Bluetooth: HCI UART protocol LL registered
[    4.272509] sdhci: Secure Digital Host Controller Interface driver
[    4.278843] sdhci: Copyright(c) Pierre Ossman
[    4.283391] sdhci-pltfm: SDHCI platform and OF driver helper
[    4.298803] caam 2100000.crypto: Entropy delay = 3200
[    4.316642] caam 2100000.crypto: Instantiated RNG4 SH0
[    4.329118] caam 2100000.crypto: Instantiated RNG4 SH1
[    4.334476] caam 2100000.crypto: device ID = 0x0a16010000000100 (Era 4)
[    4.341249] caam 2100000.crypto: job rings = 2, qi = 0
[    4.356116] sdhci-esdhc-imx 2194000.mmc: Got CD GPIO
[    4.361395] sdhci-esdhc-imx 2194000.mmc: Got WP GPIO
[    4.371240] sdhci-esdhc-imx 2198000.mmc: Got CD GPIO
[    4.377103] sdhci-esdhc-imx 2198000.mmc: Got WP GPIO
[    4.406462] caam algorithms registered in /proc/crypto
[    4.412336] caam 2100000.crypto: registering rng-caam
[    4.419975] caam 2100000.crypto: rng crypto API alg registered prng-caam
[    4.433156] usbcore: registered new interface driver usbhid
[    4.438888] usbhid: USB HID core driver
[    4.454775] random: crng init done
[    4.467263] mmc3: SDHCI controller on 219c000.mmc [219c000.mmc] using ADMA
[    4.479391] mmc2: SDHCI controller on 2198000.mmc [2198000.mmc] using ADMA
[    4.487685] mmc1: SDHCI controller on 2194000.mmc [2194000.mmc] using ADMA
[    4.497209] sgtl5000 0-000a: sgtl5000 revision 0x11
[    4.510038] sgtl5000 0-000a: Using internal LDO instead of VDDD: check ER1 erratum
[    4.532750] usb 1-1: new high-speed USB device number 2 using ci_hdrc
[    4.551261] mmc2: new high speed SDXC card at address 1234
[    4.562738] mmcblk2: mmc2:1234 SA128 116 GiB
[    4.581347]  mmcblk2: p1 p2
[    4.598741] mmc3: new DDR MMC card at address 0001
[    4.606496] mmcblk3: mmc3:0001 MMC04G 3.58 GiB
[    4.627014]  mmcblk3: p1 p2 p3 < p5 p6 p7 p8 > p4
[    4.634931] mmcblk3: p4 size 5324800 extends beyond EOD, truncated
[    4.654524] fsl-ssi-dai 2028000.ssi: No cache defaults, reading back from HW
[    4.677119] mmcblk3boot0: mmc3:0001 MMC04G 2.00 MiB
[    4.690313] mmcblk3boot1: mmc3:0001 MMC04G 2.00 MiB
[    4.704026] mmcblk3rpmb: mmc3:0001 MMC04G 128 KiB, chardev (243:0)
[    4.737312] usb 1-1: New USB device found, idVendor=1a40, idProduct=0101, bcdDevice= 1.00
[    4.745984] usb 1-1: New USB device strings: Mfr=0, Product=1, SerialNumber=0
[    4.753425] usb 1-1: Product: USB 2.0 Hub [MTT]
[    4.761082] imx-sgtl5000 sound: ASoC: driver name too long 'imx6-riotboard-sgtl5000' -> 'imx6-riotboard-'
[    4.777813] hub 1-1:1.0: USB hub found
[    4.782293] hub 1-1:1.0: 4 ports detected
[    4.808029] ipip: IPv4 and MPLS over IPv4 tunneling driver
[    4.817137] Initializing XFRM netlink socket
[    4.822090] NET: Registered PF_INET6 protocol family
[    4.831903] Segment Routing with IPv6
[    4.835909] In-situ OAM (IOAM) with IPv6
[    4.840101] sit: IPv6, IPv4 and MPLS over IPv4 tunneling driver
[    4.849380] NET: Registered PF_PACKET protocol family
[    4.854695] can: controller area network core
[    4.859366] NET: Registered PF_CAN protocol family
[    4.864375] can: raw protocol
[    4.867641] can: broadcast manager protocol
[    4.872012] can: netlink gateway - max_hops=1
[    4.878350] NET: Registered PF_KCM protocol family
[    4.883969] Key type dns_resolver registered
[    4.901078] Registering SWP/SWPB emulation handler
[    4.962700] Loading compiled-in X.509 certificates
[    5.133855] pps pps0: new PPS source ptp0
[    5.142334] fec 2188000.ethernet: Invalid MAC address: 00:00:00:00:00:00
[    5.149408] fec 2188000.ethernet: Using random MAC address: 2a:69:68:b0:18:4f
[    5.203620] fec 2188000.ethernet eth0: registered PHC device 0
[    5.237259] imx_thermal 20c8000.anatop:tempmon: Commercial CPU temperature grade - max:95C critical:90C passive:85C
[    5.256993] psci_checker: Missing PSCI operations, aborting tests
[    5.263723] cfg80211: Loading compiled-in X.509 certificates for regulatory database
[    5.278530] Loaded X.509 cert 'sforshee: 00b28ddf47aef9cea7'
[    5.285085] clk: Disabling unused clocks
[    5.289925] ALSA device list:
[    5.293077]   #0: imx6-riotboard-sgtl5000
[    5.301645] platform regulatory.0: Direct firmware load for regulatory.db failed with error -2
[    5.310643] platform regulatory.0: Falling back to sysfs fallback for: regulatory.db
[    5.334478] usb 1-1.1: new high-speed USB device number 3 using ci_hdrc
[    5.364243] EXT4-fs (mmcblk2p2): mounted filesystem 8b80a6fa-b55a-4279-949a-3c9650a45f39 ro with ordered data mode. Quota mode: none.
[    5.377152] VFS: Mounted root (ext4 filesystem) readonly on device 179:2.
[    5.388048] devtmpfs: mounted
[    5.392635] Freeing unused kernel image (initmem) memory: 1024K
[    5.399545] Run /sbin/init as init process
[    5.403856]   with arguments:
[    5.403878]     /sbin/init
[    5.403895]   with environment:
[    5.403912]     HOME=/
[    5.403927]     TERM=linux
[    5.403944]     fbmem=10M
[    5.605334] usb 1-1.1: New USB device found, idVendor=0bda, idProduct=0811, bcdDevice= 2.00
[    5.614180] usb 1-1.1: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[    5.621751] usb 1-1.1: Product: 802.11ac WLAN Adapter 
[    5.627145] usb 1-1.1: Manufacturer: Realtek 
[    5.631627] usb 1-1.1: SerialNumber: 00e04c000001
[    6.173725] systemd[1]: Failed to find module 'autofs4'
[    6.276643] systemd[1]: systemd 247.3-7+deb11u4 running in system mode. (+PAM +AUDIT +SELINUX +IMA +APPARMOR +SMACK +SYSVINIT +UTMP +LIBCRYPTSETUP +GCRYPT +GNUTLS +ACL +XZ +LZ4 +ZSTD +SECCOMP +BLKID +ELFUTILS +KMOD +IDN2 -IDN +PCRE2 default-hierarchy=unified)
[    6.305304] systemd[1]: Detected architecture arm.
[    6.365636] systemd[1]: Set hostname to <debian>.
[    8.248261] systemd[1]: /etc/systemd/system/rc-local.service:12: Support for option SysVStartPriority= has been removed and it is ignored
[    8.344289] systemd[1]: Queued start job for default target Graphical Interface.
[    8.354791] systemd[1]: system-getty.slice: unit configures an IP firewall, but the local system does not support BPF/cgroup firewalling.
[    8.367774] systemd[1]: (This warning is only shown for the first unit using IP firewalling.)
[    8.380920] systemd[1]: Created slice system-getty.slice.
[    8.428231] systemd[1]: Created slice system-modprobe.slice.
[    8.467210] systemd[1]: Created slice system-serial\x2dgetty.slice.
[    8.504866] systemd[1]: Created slice User and Session Slice.
[    8.546419] systemd[1]: Started Dispatch Password Requests to Console Directory Watch.
[    8.595718] systemd[1]: Started Forward Password Requests to Wall Directory Watch.
[    8.634541] systemd[1]: Starting of Arbitrary Executable File Formats File System Automount Point not supported.
[    8.683396] systemd[1]: Reached target Local Encrypted Volumes.
[    8.724053] systemd[1]: Reached target Network (Pre).
[    8.763071] systemd[1]: Reached target Paths.
[    8.793803] systemd[1]: Reached target Remote File Systems.
[    8.833003] systemd[1]: Reached target Slices.
[    8.863913] systemd[1]: Reached target Swap.
[    8.905449] systemd[1]: Listening on Syslog Socket.
[    8.946903] systemd[1]: Listening on fsck to fsckd communication Socket.
[    8.987143] systemd[1]: Listening on initctl Compatibility Named Pipe.
[    9.049317] systemd[1]: Condition check resulted in Journal Audit Socket being skipped.
[    9.061287] systemd[1]: Listening on Journal Socket (/dev/log).
[    9.105543] systemd[1]: Listening on Journal Socket.
[    9.169185] systemd[1]: Listening on udev Control Socket.
[    9.217656] systemd[1]: Listening on udev Kernel Socket.
[    9.254776] systemd[1]: Condition check resulted in Huge Pages File System being skipped.
[    9.293984] systemd[1]: Mounting POSIX Message Queue File System...
[    9.333581] systemd[1]: Mounting Kernel Debug File System...
[    9.397402] systemd[1]: Mounting Kernel Trace File System...
[    9.486671] systemd[1]: Starting Set the console keyboard layout...
[    9.548610] systemd[1]: Starting Create list of static device nodes for the current kernel...
[    9.634597] systemd[1]: Starting Load Kernel Module configfs...
[    9.723297] systemd[1]: Starting Load Kernel Module drm...
[    9.796300] systemd[1]: Starting Load Kernel Module fuse...
[    9.856847] systemd[1]: Condition check resulted in Set Up Additional Binary Formats being skipped.
[    9.924250] systemd[1]: Starting File System Check on Root Device...
[    9.971094] systemd[1]: Starting Journal Service...
[   10.074267] systemd[1]: Starting Load Kernel Modules...
[   10.184598] systemd[1]: Starting Coldplug All udev Devices...
[   10.336126] systemd[1]: Mounted POSIX Message Queue File System.
[   10.393977] systemd[1]: Mounted Kernel Debug File System.
[   10.473549] systemd[1]: Mounted Kernel Trace File System.
[   10.555745] systemd[1]: Finished Create list of static device nodes for the current kernel.
[   10.750161] systemd[1]: modprobe@configfs.service: Succeeded.
[   10.829182] systemd[1]: Finished Load Kernel Module configfs.
[   10.872775] 88XXau: loading out-of-tree module taints kernel.
[   10.917956] systemd[1]: modprobe@drm.service: Succeeded.
[   10.989374] systemd[1]: Finished Load Kernel Module drm.
[   11.028706] systemd[1]: modprobe@fuse.service: Succeeded.
[   11.098534] systemd[1]: Finished Load Kernel Module fuse.
[   11.193288] systemd[1]: Finished File System Check on Root Device.
[   11.333742] systemd[1]: Mounting FUSE Control File System...
[   11.453964] systemd[1]: Mounting Kernel Configuration File System...
[   11.574628] systemd[1]: Started File System Check Daemon to report status.
[   11.684388] systemd[1]: Starting Remount Root and Kernel File Systems...
[   11.792066] systemd[1]: Mounted FUSE Control File System.
[   11.904983] systemd[1]: Mounted Kernel Configuration File System.
[   12.205750] usb 1-1.1: 88XXau e8:4e:06:5b:fa:98 hw_info[107]
[   12.282578] usbcore: registered new interface driver rtl88XXau
[   12.432888] systemd[1]: Finished Load Kernel Modules.
[   12.504808] systemd[1]: Starting Apply Kernel Variables...
[   12.872048] systemd[1]: Started Journal Service.
[   14.494150] EXT4-fs (mmcblk2p2): re-mounted 8b80a6fa-b55a-4279-949a-3c9650a45f39 r/w. Quota mode: none.
[   14.994544] systemd-journald[111]: Received client request to flush runtime journal.
[   19.271937] cfg80211: failed to load regulatory.db
[   19.695236] coda 2040000.vpu: Direct firmware load for vpu_fw_imx6d.bin failed with error -2
[   19.704125] coda 2040000.vpu: Falling back to sysfs fallback for: vpu_fw_imx6d.bin
[   20.314960] imx-sdma 20ec000.dma-controller: external firmware not found, using ROM firmware
[   24.071571] coda 2040000.vpu: Using fallback firmware vpu/vpu_fw_imx6d.bin
[   24.216788] coda 2040000.vpu: Firmware code revision: 46072
[   24.222698] coda 2040000.vpu: Initialized CODA960.
[   24.227593] coda 2040000.vpu: Firmware version: 3.1.1
[   24.431601] coda 2040000.vpu: coda-jpeg-encoder registered as video0
[   24.509705] coda 2040000.vpu: coda-jpeg-decoder registered as video1
[   24.582019] coda 2040000.vpu: coda-video-encoder registered as video2
[   24.650993] coda 2040000.vpu: coda-video-decoder registered as video3
[   30.663622] Qualcomm Atheros AR8035 2188000.ethernet-1:04: attached PHY driver (mii_bus:phy_addr=2188000.ethernet-1:04, irq=56)

[   31.570231] ============================================
[   31.575572] WARNING: possible recursive locking detected
[   31.580915] 6.5.7-lcy-20231018 #1 Tainted: G           O      
[   31.586777] --------------------------------------------
[   31.592115] ip/304 is trying to acquire lock:
[   31.596497] c44f108c (pmutex){+.+.}-{3:3}, at: usbctrl_vendorreq+0x74/0x24c [88XXau]
[   31.604682] 
               but task is already holding lock:
[   31.610549] c44f00d8 (pmutex){+.+.}-{3:3}, at: netdev_open+0x2c/0x4c [88XXau]
[   31.618053] 
               other info that might help us debug this:
[   31.624615]  Possible unsafe locking scenario:

[   31.630564]        CPU0
[   31.633029]        ----
[   31.635494]   lock(pmutex);
[   31.638316]   lock(pmutex);
[   31.641137] 
                *** DEADLOCK ***

[   31.647087]  May be due to missing lock nesting notation

[   31.653907] 2 locks held by ip/304:
[   31.657419]  #0: c1be1be4 (rtnl_mutex){+.+.}-{3:3}, at: rtnetlink_rcv_msg+0x140/0x588
[   31.665334]  #1: c44f00d8 (pmutex){+.+.}-{3:3}, at: netdev_open+0x2c/0x4c [88XXau]
[   31.673274] 
               stack backtrace:
[   31.677658] CPU: 0 PID: 304 Comm: ip Tainted: G           O       6.5.7-lcy-20231018 #1
[   31.685705] Hardware name: Freescale i.MX6 Quad/DualLite (Device Tree)
[   31.692272]  unwind_backtrace from show_stack+0x10/0x14
[   31.697551]  show_stack from dump_stack_lvl+0x60/0x90
[   31.702651]  dump_stack_lvl from __lock_acquire+0x12ec/0x2990
[   31.708447]  __lock_acquire from lock_acquire.part.0+0xb4/0x264
[   31.714412]  lock_acquire.part.0 from __mutex_lock+0x9c/0x8e0
[   31.720210]  __mutex_lock from mutex_lock_nested+0x1c/0x24
[   31.725742]  mutex_lock_nested from usbctrl_vendorreq+0x74/0x24c [88XXau]
[   31.732886]  usbctrl_vendorreq [88XXau] from usb_read8+0x40/0x6c [88XXau]
[   31.740293]  usb_read8 [88XXau] from rtl8812au_hal_init+0x6c/0xfac [88XXau]
[   31.747863]  rtl8812au_hal_init [88XXau] from rtw_hal_init+0x18/0xdc [88XXau]
[   31.755610]  rtw_hal_init [88XXau] from _netdev_open+0x4c/0x200 [88XXau]
[   31.762923]  _netdev_open [88XXau] from netdev_open+0x34/0x4c [88XXau]
[   31.770053]  netdev_open [88XXau] from __dev_open+0xfc/0x1ac
[   31.776049]  __dev_open from __dev_change_flags+0x190/0x228
[   31.781665]  __dev_change_flags from dev_change_flags+0x18/0x54
[   31.787625]  dev_change_flags from do_setlink+0x384/0x1018
[   31.793155]  do_setlink from rtnl_newlink+0x510/0x948
[   31.798248]  rtnl_newlink from rtnetlink_rcv_msg+0x170/0x588
[   31.803949]  rtnetlink_rcv_msg from netlink_rcv_skb+0xb8/0x11c
[   31.809829]  netlink_rcv_skb from netlink_unicast+0x198/0x2cc
[   31.815620]  netlink_unicast from netlink_sendmsg+0x1e0/0x450
[   31.821405]  netlink_sendmsg from ____sys_sendmsg+0xc0/0x2b4
[   31.827112]  ____sys_sendmsg from ___sys_sendmsg+0x94/0xcc
[   31.832643]  ___sys_sendmsg from sys_sendmsg+0x70/0xb8
[   31.837824]  sys_sendmsg from ret_fast_syscall+0x0/0x1c
[   31.843091] Exception stack(0xe6bf9fa8 to 0xe6bf9ff0)
[   31.848174] 9fa0:                   00000000 bec0c7a4 00000003 bec0c720 00000000 00000000
[   31.856395] 9fc0: 00000000 bec0c7a4 b6fd4df0 00000128 652f5dd9 00000000 00000000 0055ac44
[   31.864612] 9fe0: 00000128 bec0c6c8 b6e86f1f b6e007e6
[   33.393878] 8021q: 802.1Q VLAN Support v1.8
[   33.765937] fec 2188000.ethernet eth0: Link is Up - 1Gbps/Full - flow control rx/tx
[   35.266721] bridge: filtering via arp/ip/ip6tables is no longer available by default. Update your scripts to load br_netfilter if you need this.
[   35.530870] br-lan: port 1(eth0) entered blocking state
[   35.536339] br-lan: port 1(eth0) entered disabled state
[   35.541660] fec 2188000.ethernet eth0: entered allmulticast mode
[   35.642673] fec 2188000.ethernet eth0: entered promiscuous mode
[   35.827573] br-lan: port 1(eth0) entered blocking state
[   35.832976] br-lan: port 1(eth0) entered forwarding state
[   36.414155] tun: Universal TUN/TAP device driver, 1.6
[   37.218948] br-lan: port 2(wlan0) entered blocking state
[   37.224495] br-lan: port 2(wlan0) entered disabled state
[   37.230221] rtl88XXau 1-1.1:1.0 wlan0: entered allmulticast mode
[   37.236973] rtl88XXau 1-1.1:1.0 wlan0: entered promiscuous mode
[   37.243126] br-lan: port 2(wlan0) entered blocking state
[   37.248483] br-lan: port 2(wlan0) entered forwarding state
[   62.408176] systemd[609]: memfd_create() called without MFD_EXEC or MFD_NOEXEC_SEAL set
[   82.046163] usb 1-1.4: new high-speed USB device number 4 using ci_hdrc
[   82.308453] usb 1-1.4: New USB device found, idVendor=148f, idProduct=7601, bcdDevice= 0.00
[   82.317105] usb 1-1.4: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[   82.324608] usb 1-1.4: Product: 802.11 n WLAN
[   82.329217] usb 1-1.4: Manufacturer: MediaTek
[   82.333664] usb 1-1.4: SerialNumber: 1.0
[   82.575915] usb 1-1.4: reset high-speed USB device number 4 using ci_hdrc
[   82.829937] mt7601u 1-1.4:1.0: ASIC revision: 76010001 MAC revision: 76010500
[   82.856766] mt7601u 1-1.4:1.0: Firmware Version: 0.1.00 Build: 7640 Build time: 201302052146____
[   83.327514] mt7601u 1-1.4:1.0: EEPROM ver:0d fae:00
[   83.618608] ieee80211 phy1: Selected rate control algorithm 'minstrel_ht'

```

## Issues

* There will be a deadlock when loading the 88XXau module, but it does not affect the use. I have tested it and confirmed that it is a problem with 88XXau.



## Credits

I would like to thank the Kail-ARM project for inspiring me.
