注1：学习一个开发板的目的，就是像一个产品经理那样，不断添加新的功能，看怎么把它的cpu用到100%，榨干它的性能。

注2:有一个百问网100ask的imx6ull开发板的wiki,链接[:]([百问网imx6ull开发板 - Ubuntu中文](https://wiki.ubuntu.org.cn/%E7%99%BE%E9%97%AE%E7%BD%91imx6ull%E5%BC%80%E5%8F%91%E6%9D%BF))


# 硬件

//CPU
![image](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/69979e1bcdd44b3a88112d62ff3b5d41~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=1878\&h=1619\&s=178266\&e=png\&b=fdfdfd)

//百问开发板：
![image](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/80b4a0082e4d431fb64c7286368d2e56~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=949\&h=1227\&s=1197280\&e=png\&b=fbf8f8)

# uboot

## uboot支持的命令

```

=> help
?       - alias for 'help'
base    - print or set address offset
bdinfo  - print Board Info structure
bmode   - sd1|sd2|qspi1|normal|usb|sata|ecspi1:0|ecspi1:1|ecspi1:2|ecspi1:3|esdhc1|esdhc2|esdhc3|esdhc4 [noreset]
bmp     - manipulate BMP image data
boot    - boot default, i.e., run 'bootcmd'
bootd   - boot default, i.e., run 'bootcmd'
bootefi - Boots an EFI payload from memory
bootelf - Boot from an ELF image in memory
bootm   - boot application image from memory
bootp   - boot image via network using BOOTP/TFTP protocol
bootvx  - Boot vxWorks from an ELF image
bootz   - boot Linux zImage image from memory
clocks  - display clocks
clrlogo - fill the boot logo area with black
cmp     - memory compare
coninfo - print console devices and information
cp      - memory copy
crc32   - checksum calculation
dcache  - enable or disable data cache
dhcp    - boot image via network using DHCP/TFTP protocol
dm      - Driver model low level access
echo    - echo args to console
editenv - edit environment variable
env     - environment handling commands
erase   - erase FLASH memory
exit    - exit script
ext2load- load binary file from a Ext2 filesystem
ext2ls  - list files in a directory (default /)
ext4load- load binary file from a Ext4 filesystem
ext4ls  - list files in a directory (default /)
ext4size- determine a file's size
ext4write- create a file in the root directory
false   - do nothing, unsuccessfully
fatinfo - print information about filesystem
fatload - load binary file from a dos filesystem
fatls   - list files in a directory (default /)
fatsize - determine a file's size
fdt     - flattened device tree utility commands
flinfo  - print FLASH memory information
fstype  - Look up a filesystem type
fuse    - Fuse sub-system
go      - start application at address 'addr'
gpio    - query and control gpio pins
help    - print command description/usage
i2c     - I2C sub-system
icache  - enable or disable instruction cache
iminfo  - print header information for application image
imxtract- extract a part of a multi-image
itest   - return true/false on integer compare
load    - load binary file from a filesystem
loadb   - load binary file over serial line (kermit mode)
loads   - load S-Record file over serial line
loadx   - load binary file over serial line (xmodem mode)
loady   - load binary file over serial line (ymodem mode)
loop    - infinite loop on address range
ls      - list files in a directory (default /)
md      - memory display
mdio    - MDIO utility commands
mii     - MII utility commands
mm      - memory modify (auto-incrementing address)
mmc     - MMC sub system
mmcinfo - display MMC info
mtest   - simple RAM read/write test
mw      - memory write (fill)
nfs     - boot image via network using NFS protocol
nm      - memory modify (constant address)
ping    - send ICMP ECHO_REQUEST to network host
printenv- print environment variables
protect - enable or disable FLASH write protection
reset   - Perform RESET of the CPU
run     - run commands in an environment variable
save    - save file to a filesystem
saveenv - save environment variables to persistent storage
setenv  - set environment variables
setexpr - set environment variable as the result of eval expression
sf      - SPI flash sub-system
showvar - print local hushshell variables
size    - determine a file's size
sleep   - delay execution for some time
source  - run script from memory
test    - minimal test like /bin/sh
tftpboot- boot image via network using TFTP protocol
true    - do nothing, successfully
usb     - USB sub-system
usbboot - boot from USB device
version - print monitor, compiler and linker version
=>

```

## uboot环境变量

```
=>printenv
baudrate=115200
board_name=EVK
board_rev=14X14
boot_fdt=try
bootcmd=run updateset;run findfdt;run findtee;mmc dev ${mmcdev};mmc dev ${mmcdev}; if mmc rescan; then if run loadbootscript; then run bootscript; else if run loadimage; then run mmcboot; else run netboot; fi; fi; else run netboot; fi
bootcmd_mfg=run mfgtool_args; if test ${tee} = yes; then bootm ${tee_addr} ${initrd_addr} ${fdt_addr}; else bootz ${loadaddr} ${initrd_addr} ${fdt_addr}; fi;
bootdelay=3
bootdir=/boot
bootscript=echo Running bootscript from mmc ...; source
console=ttymxc0
eth1addr=00:01:3f:2d:3e:4d
ethaddr=00:01:1f:2d:3e:4d
ethprime=eth1
fdt_addr=0x83000000
fdt_file=100ask_imx6ull-14x14.dtb
fdt_high=0xffffffff
fdtcontroladdr=9ef40478
findfdt=if test $fdt_file = undefined; then if test $board_name = EVK && test $board_rev = 9X9; then setenv fdt_file imx6ull-9x9-evk.dtb; fi; if test $board_name = EVK && test $board_rev = 14X14; then setenv fdt_file imx6ull-14x14-evk.dtb; fi; if test $fdt_file = undefined; then setenv fdt_file imx6ull-14x14-alpha.dtb; fi; fi;
image=zImage
initrd_addr=0x83800000
initrd_high=0xffffffff
ip_dyn=yes
loadaddr=0x80800000
loadbootscript=fatload mmc ${mmcdev}:${mmcpart} ${loadaddr} ${script};
loadfdt=ext2load mmc ${mmcdev}:${mmcpart} ${fdt_addr} ${bootdir}/${fdt_file}
loadimage=ext2load mmc ${mmcdev}:${mmcpart} ${loadaddr} ${bootdir}/${image}
loadtee=fatload mmc ${mmcdev}:${mmcpart} ${tee_addr} ${tee_file}
mfgtool_args=setenv bootargs console=${console},${baudrate} rdinit=/linuxrc g_mass_storage.stall=0 g_mass_storage.removable=1 g_mass_storage.file=/fat g_mass_storage.ro=1 g_mass_storage.idVendor=0x066F g_mass_storage.idProduct=0x37FF g_mass_storage.iSerialNumber="" clk_ignore_unused
mmcargs=setenv bootargs console=${console},${baudrate} root=${mmcroot}
mmcautodetect=no
mmcboot=echo Booting from mmc ...; run mmcargs; if test ${tee} = yes; then run loadfdt; run loadtee; bootm ${tee_addr} - ${fdt_addr}; else if test ${boot_fdt} = yes || test ${boot_fdt} = try; then if run loadfdt; then bootz ${loadaddr} - ${fdt_addr}; else if test ${boot_fdt} = try; then bootz; else echo WARN: Cannot load the DT; fi; fi; else bootz; fi; fi;
mmcdev=1
mmcpart=2
mmcroot=/dev/mmcblk1p2 rootwait rw
netargs=setenv bootargs console=${console},${baudrate} root=/dev/nfs ip=dhcp nfsroot=${serverip}:${nfsroot},v3,tcp
netboot=echo Booting from net ...; run netargs; setenv get_cmd tftp; ${get_cmd} ${image}; ${get_cmd} ${fdt_addr} ${fdt_file};  bootz ${loadaddr} - ${fdt_addr};
panel=TFT7016
script=boot.scr
tee=no
tee_addr=0x84000000
tee_file=uTee-6ullevk
update=yes
updateset=if test $update = undefined; then setenv update yes; saveenv; fi;

Environment size: 2738/8188 bytes
=>

```

## I2C

    => i2c bus 0
    Bus 0:  i2c@021a0000  (active 0)
    => i2c bus 1
    Bus 1:  i2c@021a4000  (active 1)

## SPI接口flash

    => sf probe
    SF: unrecognized JEDEC id bytes: ff, ff, ff
    Failed to initialize SPI flash at 0:0 (error -2)

## 网卡相关

    => mii info
    NULL device name!
    No such device: <NULL>
    NULL device name!
    No such device: <NULL>

## mmc相关

```

=> mmc list
FSL_SDHC: 0 (SD)
FSL_SDHC: 1 (eMMC)
=> mmc dev 0    //选中0 = SD卡
switch to partitions #0, OK
mmc0 is current device
=> mmc info     //SD卡信息
Device: FSL_SDHC
Manufacturer ID: 9f
OEM: 5449
Name: SD32G
Bus Speed: 50000000
Mode : SD High Speed (50MHz)
Rd Block Len: 512
SD version 3.0
High Capacity: Yes
Capacity: 29.1 GiB
Bus Width: 4-bit
Erase Group Size: 512 Bytes
=> mmc part      //SD卡分区

Partition Map for MMC device 0  --   Partition Type: DOS

Part    Start Sector    Num Sectors     UUID            Type
  1     1024            20480           4bab40be-01     0c
  2     21504           8388608         4bab40be-02     83
=> mmc dev 1       //选中1 = MMC
switch to partitions #0, OK
mmc1(part 0) is current device
=> mmc info        //MMC的信息
Device: FSL_SDHC
Manufacturer ID: 13
OEM: 14e
Name: Q2J54
Bus Speed: 52000000
Mode : MMC High Speed (52MHz)
Rd Block Len: 512
MMC version 5.0
High Capacity: Yes
Capacity: 3.6 GiB
Bus Width: 4-bit
Erase Group Size: 512 KiB
HC WP Group Size: 8 MiB
User Capacity: 3.6 GiB WRREL
Boot Capacity: 2 MiB ENH
RPMB Capacity: 512 KiB ENH
=> mmc part        //MMC的分区

Partition Map for MMC device 1  --   Partition Type: DOS

Part    Start Sector    Num Sectors     UUID            Type
  1     20480           102400          00000000-01     0c
  2     122880          4194304         00000000-02     83
=>

```

# linux下常用命令

## 7zip基准跑分

```

[root@imx6ull:~]# ./7zzs  b

7-Zip (z) 21.07 (armt) : Copyright (c) 1999-2021 Igor Pavlov : 2021-12-26
 32-bit arm_v:7 thumb:2 locale=C UTF8=- Threads:1

Compiler: 9.2.1 20191025 GCC 9.2.1
Linux : 4.9.88 : #1 SMP PREEMPT Wed Apr 22 15:53:26 CST 2020 : armv7l
PageSize:4KB hwcap:1FB0D6:NEON hwcap2:0
LE

1T CPU Freq (MHz):   714   782   783   782   781   780   783

RAM size:     491 MB,  # CPU hardware threads:   1
RAM usage:    437 MB,  # Benchmark threads:      1

                       Compressing  |                  Decompressing
Dict     Speed Usage    R/U Rating  |      Speed Usage    R/U Rating
         KiB/s     %   MIPS   MIPS  |      KiB/s     %   MIPS   MIPS

22:        458   100    448    446  |       7638   100    654    652
23:        431   100    441    439  |       7520   100    653    651
24:        413   100    446    444  |       7396   100    652    649
[ 1363.145269] 7zzs invoked oom-killer: gfp_mask=0x26080c2(GFP_KERNEL|__GFP_HIGHMEM|__GFP_ZERO|__GFP_NOTRACK), nodemask=0, order=0, oom_score_adj=0
...//crash 了，上面跑分可以参考对比
```

## 内核配置 zcat /proc/config.gz
```

[root@imx6ull:~]# zcat /proc/config.gz
#
# Automatically generated file; DO NOT EDIT.
# Linux/arm 4.9.88 Kernel Configuration
#
CONFIG_ARM=y
CONFIG_ARM_HAS_SG_CHAIN=y
CONFIG_MIGHT_HAVE_PCI=y
CONFIG_SYS_SUPPORTS_APM_EMULATION=y
.......
# CONFIG_VHOST_NET is not set
# CONFIG_VHOST_CROSS_ENDIAN_LEGACY is not set
[root@imx6ull:~]#

```

## busybox 支持的命令

```
[root@imx6ull:~]# busybox --help
BusyBox v1.29.3 (2020-04-22 14:57:46 CST) multi-call binary.
BusyBox is copyrighted by many authors between 1998-2015.
Licensed under GPLv2. See source distribution for detailed
copyright notices.

Usage: busybox [function [arguments]...]
   or: busybox --list[-full]
   or: busybox --install [-s] [DIR]
   or: function [arguments]...

        BusyBox is a multi-call binary that combines many common Unix
        utilities into a single executable.  Most people will create a
        link to busybox for each function they wish to use and BusyBox
        will act like whatever it was invoked as.

Currently defined functions:
        [, [[, addgroup, adduser, ar, arch, arp, arping, ash, awk, basename, blkid, bunzip2, bzcat, cat, chattr, chgrp, chmod, chown, chroot, chrt, chvt, cksum, clear, cmp, cp,
        cpio, crond, crontab, cut, date, dc, dd, deallocvt, delgroup, deluser, depmod, devmem, df, diff, dirname, dmesg, dnsd, dnsdomainname, dos2unix, du, dumpkmap, echo, egrep,
        eject, env, ether-wake, expr, factor, fallocate, false, fbset, fdflush, fdformat, fdisk, fgrep, find, flock, fold, free, freeramdisk, fsck, fsfreeze, fstrim, fuser, getopt,
        getty, grep, gunzip, gzip, halt, hdparm, head, hexdump, hexedit, hostid, hostname, hwclock, i2cdetect, i2cdump, i2cget, i2cset, id, ifconfig, ifdown, ifup, inetd, init,
        insmod, install, ip, ipaddr, ipcrm, ipcs, iplink, ipneigh, iproute, iprule, iptunnel, kill, killall, killall5, klogd, last, less, link, linux32, linux64, linuxrc, ln,
        loadfont, loadkmap, logger, login, logname, losetup, ls, lsattr, lsmod, lsof, lspci, lsscsi, lsusb, lzcat, lzma, lzopcat, makedevs, md5sum, mdev, mesg, microcom, mkdir,
        mkdosfs, mke2fs, mkfifo, mknod, mkpasswd, mkswap, mktemp, modinfo, modprobe, more, mount, mountpoint, mt, mv, nameif, netstat, nice, nl, nohup, nproc, nslookup, nuke, od,
        openvt, partprobe, passwd, paste, patch, pidof, ping, pipe_progress, pivot_root, poweroff, printenv, printf, ps, pwd, rdate, readlink, readprofile, realpath, reboot, renice,
        reset, resize, resume, rm, rmdir, rmmod, route, run-init, run-parts, runlevel, sed, seq, setarch, setconsole, setfattr, setkeycodes, setlogcons, setpriv, setserial, setsid,
        sh, sha1sum, sha256sum, sha3sum, sha512sum, shred, sleep, sort, start-stop-daemon, strings, stty, su, sulogin, svc, svok, swapoff, swapon, switch_root, sync, sysctl,
        syslogd, tail, tar, tc, tee, telnet, test, tftp, time, top, touch, tr, traceroute, true, truncate, tty, ubirename, udhcpc, uevent, umount, uname, uniq, unix2dos, unlink,
        unlzma, unlzop, unxz, unzip, uptime, usleep, uudecode, uuencode, vconfig, vi, vlock, w, watch, watchdog, wc, wget, which, who, whoami, xargs, xxd, xz, xzcat, yes, zcat
[root@imx6ull:~]#
[root@imx6ull:~]#

```

## busybox下读取设备树的节点值的方法
```
    [root@imx6ull:/proc/device-tree]# hexdump ./memory/reg
    0000000 0080 0000 0020 0000
    0000008
```

## \[root\@imx6ull:/sys]# lsmod
```
    Module                  Size  Used by
    inv_mpu6050_spi         2052  0
    inv_mpu6050            10948  2 inv_mpu6050_spi
    evbug                   2078  0
    100ask_adxl345_spi      3963  0
    100ask_spidev           9333  0
    100ask_irda             3442  0
    100ask_rc_nec           1146  0
    100ask_dht11            3948  0
    100ask_ds18b20          4174  0
    8723bu               1823356  0
    [root@imx6ull:/sys]#
```
## \[root\@imx6ull:/proc]# uname -a

    Linux imx6ull 4.9.88 #1 SMP PREEMPT Wed Apr 22 15:53:26 CST 2020 armv7l GNU/Linux


## \[root\@imx6ull:/proc]# df -h

    Filesystem                Size      Used Available Use% Mounted on
    /dev/root                 1.9G    433.6M      1.4G  23% /
    devtmpfs                 85.3M         0     85.3M   0% /dev
    tmpfs                   245.8M         0    245.8M   0% /dev/shm
    tmpfs                   245.8M      1.6M    244.2M   1% /tmp
    tmpfs                   245.8M    168.0K    245.6M   0% /run

## \[root\@imx6ull:/proc]#  fdisk -l  （插32G  sdcard）

```
Disk /dev/mmcblk0: 29 GB, 31267487744 bytes, 61069312 sectors   //32GB的SD卡信息
954208 cylinders, 4 heads, 16 sectors/track
Units: sectors of 1 * 512 = 512 bytes

Device       Boot StartCHS    EndCHS        StartLBA     EndLBA    Sectors  Size Id Type
/dev/mmcblk0p1    0,16,17     1,86,21           1024      21503      20480 10.0M  c Win95 FAT32 (LBA)
/dev/mmcblk0p2    1,86,22     523,128,53       21504    8410111    8388608 4096M 83 Linux

Disk /dev/mmcblk1: 3728 MB, 3909091328 bytes, 7634944 sectors      //4GB的mmc信息
119296 cylinders, 4 heads, 16 sectors/track
Units: sectors of 1 * 512 = 512 bytes

Device       Boot StartCHS    EndCHS        StartLBA     EndLBA    Sectors  Size Id Type
/dev/mmcblk1p1    1,70,6      7,165,30         20480     122879     102400 50.0M  c Win95 FAT32 (LBA)
/dev/mmcblk1p2    7,165,31    268,186,46      122880    4317183    4194304 2048M 83 Linux

Disk /dev/mmcblk1boot1: 2 MB, 2097152 bytes, 4096 sectors       //mmcblk1boot1 分区信息
64 cylinders, 4 heads, 16 sectors/track
Units: sectors of 1 * 512 = 512 bytes

Disk /dev/mmcblk1boot1 doesn't contain a valid partition table

Disk /dev/mmcblk1boot0: 2 MB, 2097152 bytes, 4096 sectors       //mmcblk1boot0 分区信息
64 cylinders, 4 heads, 16 sectors/track
Units: sectors of 1 * 512 = 512 bytes

Disk /dev/mmcblk1boot0 doesn't contain a valid partition table

```

## \[root\@imx6ull:/proc]#  fdisk -l  （插8G  sdcard）

```

[root@100ask:~]# fdisk -l
Disk /dev/mmcblk0: 7388 MB, 7746879488 bytes, 15130624 sectors    #8G 的sdcard
236416 cylinders, 4 heads, 16 sectors/track
Units: sectors of 1 * 512 = 512 bytes

Device       Boot StartCHS    EndCHS        StartLBA     EndLBA    Sectors  Size Id Type
/dev/mmcblk0p1 *  0,65,4      127,187,62        4098    2052097    2048000 1000M 83 Linux
/dev/mmcblk0p2 *  127,187,63  318,244,56     2052098    5124097    3072000 1500M 83 Linux
/dev/mmcblk0p3    318,244,57  382,178,54     5124098    6148097    1024000  500M  c Win95 FAT32 (LBA)
Disk /dev/mmcblk1: 3728 MB, 3909091328 bytes, 7634944 sectors      //4GB的mmc信息
119296 cylinders, 4 heads, 16 sectors/track
Units: sectors of 1 * 512 = 512 bytes

Device       Boot StartCHS    EndCHS        StartLBA     EndLBA    Sectors  Size Id Type
/dev/mmcblk1p1 *  0,65,4      127,187,62        4098    2052097    2048000 1000M 83 Linux
/dev/mmcblk1p2 *  127,187,63  318,244,56     2052098    5124097    3072000 1500M 83 Linux
/dev/mmcblk1p3    318,244,57  382,178,54     5124098    6148097    1024000  500M  c Win95 FAT32 (LBA)
Disk /dev/mmcblk1boot1: 2 MB, 2097152 bytes, 4096 sectors
64 cylinders, 4 heads, 16 sectors/track
Units: sectors of 1 * 512 = 512 bytes

Disk /dev/mmcblk1boot1 doesn't contain a valid partition table
Disk /dev/mmcblk1boot0: 2 MB, 2097152 bytes, 4096 sectors
64 cylinders, 4 heads, 16 sectors/track
Units: sectors of 1 * 512 = 512 bytes

Disk /dev/mmcblk1boot0 doesn't contain a valid partition table
```

![image](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a8decd2805eb44d985e3b8485bbb8cf7~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=2485\&h=203\&s=9481\&e=png\&b=d4faed)

![image](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/36be0bf568284a7fa7e26cf937649ca9~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=3000\&h=2000\&s=1037217\&e=png\&b=f6f4f3)

## \[root\@imx6ull:\~]# lsblk   //默认mmc中的linux系统不支持

    -sh: lsblk: command not found
    


## ps -ef
```
PID   USER     COMMAND
    1 root     init [3]
    2 root     [kthreadd]
    3 root     [ksoftirqd/0]
    5 root     [kworker/0:0H]
    7 root     [rcu_preempt]
    8 root     [rcu_sched]
    9 root     [rcu_bh]
   10 root     [migration/0]
   11 root     [lru-add-drain]
   12 root     [cpuhp/0]
   13 root     [kdevtmpfs]
   14 root     [oom_reaper]
   15 root     [writeback]
   16 root     [kcompactd0]
   17 root     [crypto]
   18 root     [bioset]
   19 root     [kblockd]
   20 root     [kworker/0:1]
   21 root     [ata_sff]
   22 root     [cfg80211]
   23 root     [watchdogd]
   24 root     [rpciod]
   25 root     [xprtiod]
   26 root     [kswapd0]
   27 root     [vmstat]
   28 root     [nfsiod]
   74 root     [bioset]
   75 root     [bioset]
   76 root     [bioset]
   77 root     [bioset]
   78 root     [bioset]
   79 root     [bioset]
   80 root     [bioset]
   81 root     [bioset]
   82 root     [bioset]
   83 root     [bioset]
   84 root     [bioset]
   85 root     [bioset]
   86 root     [bioset]
   87 root     [bioset]
   88 root     [bioset]
   89 root     [bioset]
   90 root     [bioset]
   91 root     [bioset]
   92 root     [bioset]
   93 root     [bioset]
   94 root     [bioset]
   95 root     [bioset]
   96 root     [bioset]
   97 root     [bioset]
   98 root     [spi32766]
   99 root     [spi0]
  100 root     [spi2]
  101 root     [kworker/u2:1]
  104 root     [ci_otg]
  106 root     [kworker/u3:0]
  107 root     [hci0]
  108 root     [hci0]
  110 root     [cfinteractive]
  111 root     [kworker/u3:2]
  112 root     [irq/64-mmc0]
  113 root     [irq/98-2190000.]
  114 root     [irq/65-mmc1]
  115 root     [mxs_dcp_chan/sh]
  116 root     [mxs_dcp_chan/ae]
  121 root     [bioset]
  123 root     [mmcqd/0]
  124 root     [bioset]
  125 root     [mmcqd/1]
  126 root     [bioset]
  127 root     [mmcqd/1boot0]
  128 root     [bioset]
  129 root     [mmcqd/1boot1]
  130 root     [bioset]
  131 root     [mmcqd/1rpmb]
  132 root     [kworker/u2:2]
  133 root     [ipv6_addrconf]
  134 root     [krfcommd]
  135 root     [pxp_dispatch]
  136 root     [irq/44-imx_ther]
  137 root     [kworker/0:1H]
  138 root     [jbd2/mmcblk0p2-]
  139 root     [ext4-rsv-conver]
  173 root     /sbin/syslogd -m 0
  176 root     /sbin/klogd
  187 root     /usr/bin/dbus-daemon --syslog --fork --print-pid 4 --print-address 6 --session
  188 root     /home/myir/mxbackend
  189 root     /home/myir/mxapp --platform linuxfb
  202 root     /sbin/udevd -d
  234 dbus     dbus-daemon --system
  265 root     /usr/sbin/connmand -n
  269 root     /usr/sbin/ofonod -n
  284 root     /usr/sbin/ntpd -g
  288 root     /usr/sbin/wpa_supplicant -u
  294 root     nginx: master process /usr/sbin/nginx
  295 www-data nginx: worker process
  299 root     /usr/sbin/sshd
  311 root     smbd -D
  314 root     {smbd-notifyd} smbd -D
  315 root     {cleanupd} smbd -D
  316 root     {lpqd} smbd -D
  317 root     nmbd -D
  322 root     -sh
  342 root     [kworker/0:0]
  343 root     [kworker/0:2]
  349 root     ps -ef
[root@imx6ull:/sys/devices/virtual/thermal]#
```

## top命令

```
[root@100ask:~]# top
Mem: 162040K used, 338908K free, 612K shrd, 5092K buff, 40712K cached
CPU:   0% usr  16% sys   0% nic  83% idle   0% io   0% irq   0% sirq
Load average: 0.17 0.20 0.09 1/125 355
  PID  PPID USER     STAT   VSZ %VSZ %CPU COMMAND
  355   325 root     R     3460   1%  12% top
  228     1 root     S    53332  11%   4% /usr/share/100ask_desktop/100ask_lvgl_Main
  286     1 root     S<   98948  20%   0% /usr/bin/pulseaudio --system --daemonize --disallow-module-loading --disallow-exit --exit-idle-time=-1 --use-pid-file --disable-shm
  266     1 root     S    53100  11%   0% /usr/sbin/NetworkManager
  323     1 root     S    46864   9%   0% /usr/bin/swupdate -f /etc/swupdate/swupdate.cfg -L -e rootfs,rootfs-1 -u -c 1 -i BrImx6ullpro_01-51fa0c600
  258     1 root     S    41708   8%   0% /usr/sbin/ModemManager
  327   323 root     S    20360   4%   0% /usr/bin/swupdate -f /etc/swupdate/swupdate.cfg -L -e rootfs,rootfs-1 -u -c 1 -i BrImx6ullpro_01-51fa0c600
  296   286 root     S    20028   4%   0% /usr/libexec/pulse/gsettings-helper
  178     1 root     S    12036   2%   0% /sbin/udevd -d
  275     1 root     S     7900   2%   0% /usr/sbin/ntpd -g
  293     1 root     S     7536   2%   0% /usr/sbin/wpa_supplicant -u
  304     1 root     S     6364   1%   0% /usr/sbin/sshd
  280     1 mosquitt S     5412   1%   0% /usr/sbin/mosquitto -c /etc/mosquitto/mosquitto.conf
  217     1 dbus     S     3616   1%   0% dbus-daemon --system
    1     0 root     S     3460   1%   0% init
  156     1 root     S     3460   1%   0% /sbin/syslogd -n
  160     1 root     S     3460   1%   0% /sbin/klogd -n
  309     1 root     S     3460   1%   0% /usr/sbin/telnetd -F
  325     1 root     S     3388   1%   0% -bash
  224     1 root     S     3384   1%   0% /usr/bin/dbus-daemon --syslog --fork --print-pid 4 --print-address 6 --session
  336     1 root     S     3384   1%   0% /usr/bin/dbus-daemon --syslog --fork --print-pid 4 --print-address 6 --session
  320     1 root     S     1724   0%   0% /usr/bin/swupdate-progress -w -r
  125     2 root     SW       0   0%   0% [mmcqd/1]
  290     2 root     SW       0   0%   0% [RTW_CMD_THREAD]
  105     2 root     SW       0   0%   0% [kworker/0:2]
   20     2 root     SW       0   0%   0% [kworker/0:1]
    7     2 root     SW       0   0%   0% [rcu_preempt]
    3     2 root     SW       0   0%   0% [ksoftirqd/0]
  138     2 root     SW       0   0%   0% [jbd2/mmcblk1p2-]
  127     2 root     SW       0   0%   0% [mmcqd/1boot0]
  137     2 root     SW<      0   0%   0% [kworker/0:1H]
  106     2 root     SW<      0   0%   0% [kworker/u3:0]
  123     2 root     SW       0   0%   0% [mmcqd/0]
  129     2 root     SW       0   0%   0% [mmcqd/1boot1]
    6     2 root     SW       0   0%   0% [kworker/u2:0]
   13     2 root     SW       0   0%   0% [kdevtmpfs]
  338     2 root     SW       0   0%   0% [kworker/u2:4]
    2     0 root     SW       0   0%   0% [kthreadd]
  111     2 root     SW<      0   0%   0% [kworker/u3:2]
    4     2 root     SW       0   0%   0% [kworker/0:0]
    5     2 root     SW<      0   0%   0% [kworker/0:0H]
    8     2 root     SW       0   0%   0% [rcu_sched]
    9     2 root     SW       0   0%   0% [rcu_bh]
   10     2 root     SW       0   0%   0% [migration/0]
   11     2 root     SW<      0   0%   0% [lru-add-drain]
   12     2 root     SW       0   0%   0% [cpuhp/0]
   14     2 root     SW       0   0%   0% [oom_reaper]
   15     2 root     SW<      0   0%   0% [writeback]
[root@100ask:~]#

```

## i2c的拓扑结构

### 查询几个i2c的bus    [root@imx6ull:~]# i2cdetect -l
```
    i2c-1   i2c             21a4000.i2c                             I2C adapter
    i2c-0   i2c             21a0000.i2c                             I2C adapter
```
每个bus下挂几个外设.


### [root@imx6ull:~]# i2cdetect -y 0
```
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- 1e --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --
```
其中：
fxls8471@1e  是 3-Axis, Linear Accelerometer
mag3110@0e 飞思卡尔磁力计MAG3110  （没有检测到？？）


### [root@imx6ull:~]# i2cdetect -y 1
```
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- -- 
10: -- -- -- -- -- -- -- -- -- -- UU -- -- -- -- -- 
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
30: -- -- -- -- -- -- -- -- -- UU -- -- -- -- -- -- 
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
60: 60 -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
70: -- -- -- -- -- -- -- --                          
```
其中：
wm8960@1a 是 codec
sii902x@39 是hdmi-transmitter	
ov5640@3c (设备树文件有，没有检测到？)
0x60 设备树文件没有，是什么设备？

可以查询i2c设备的寄存器，比如bus=0的0x1e设备：
```
    [root@imx6ull:~]# i2cdump -f -y 0 0x1e
    No size specified (using byte-data access)
         0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f    0123456789abcdef
    00: 00 00 00 02 02 01 ee ee 00 00 00 00 00 00 00 00    ...?????........
    10: 00 01 00 03 ee ee ee ee ee 40 00 00 ff ff ee ee    .?.??????@....??
    20: 05 13 01 00 00 ee ee ee 00 00 00 80 00 80 ee ee    ???..???...?.???
    30: 00 00 ee 05 ee 00 ee 1b 00 00 00 00 00 00 00 00    ..???.??........
    40: 84 86 81 87 81 80 82 b1 ee ee ee ee ee ee ee ee    ????????????????
    50: ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee    ????????????????
    60: ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee    ????????????????
    70: ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee    ????????????????
    80: ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee    ????????????????
    90: ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee    ????????????????
    a0: ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee    ????????????????
    b0: ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee    ????????????????
    c0: ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee    ????????????????
    d0: ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee    ????????????????
    e0: ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee    ????????????????
    f0: ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee ee    ????????????????
```

## 文件系统


### \[root\@imx6ull:/dev]# ls -al /dev
```
[root@imx6ull:/dev]# ls -al
total 1
drwxr-xr-x   12 root     root          3400 Jan  1 00:00 .
drwxr-xr-x   24 root     root          1024 Jan  1 00:00 ..
crw-------    1 root     root      241,   0 Jan  1 00:00 adxl345
crw-------    1 root     root       10, 235 Jan  1 00:00 autofs
drwxr-xr-x    2 root     root           720 Jan  1 00:00 block
drwxr-xr-x    3 root     root            60 Jan  1 00:00 bus
drwxr-xr-x    2 root     root          2660 Jan  1 00:00 char
crw-------    1 root     root        5,   1 Jan  1 00:00 console
crw-------    1 root     root       10,  62 Jan  1 00:00 cpu_dma_latency
crw-------    1 root     root      244,   0 Jan  1 00:00 dht11
drwxr-xr-x    5 root     root           100 Jan  1 00:00 disk
drwxr-xr-x    3 root     root            80 Jan  1 00:00 dri
crw-------    1 root     root      245,   0 Jan  1 00:00 ds18b20
crw-rw----    1 root     video      29,   0 Jan  1 00:00 fb0
crw-rw----    1 root     video      29,   1 Jan  1 00:00 fb1
lrwxrwxrwx    1 root     root            13 Jan  1 00:00 fd -> /proc/self/fd
crw-rw-rw-    1 root     root        1,   7 Jan  1 00:00 full
crw-rw-rw-    1 root     root       10, 229 Jan  1 00:00 fuse
crw-------    1 root     root      254,   0 Jan  1 00:00 gpiochip0
crw-------    1 root     root      254,   1 Jan  1 00:00 gpiochip1
crw-------    1 root     root      254,   2 Jan  1 00:00 gpiochip2
crw-------    1 root     root      254,   3 Jan  1 00:00 gpiochip3
crw-------    1 root     root      254,   4 Jan  1 00:00 gpiochip4
crw-------    1 root     root      254,   5 Jan  1 00:00 gpiochip5
crw-------    1 root     root       10, 183 Jan  1 00:00 hwrng
crw-------    1 root     root       89,   0 Jan  1 00:00 i2c-0
crw-------    1 root     root       89,   1 Jan  1 00:00 i2c-1
crw-------    1 root     root      250,   0 Jan  1 00:00 iio:device0
crw-------    1 root     root      250,   1 Jan  1 00:00 iio:device1
drwxr-xr-x    3 root     root           120 Jan  1 00:00 input
crw-------    1 root     root      243,   0 Jan  1 00:00 irda
crw-r--r--    1 root     root        1,  11 Jan  1 00:00 kmsg
srw-rw-rw-    1 root     root             0 Jan  1 00:00 log
crw-rw----    1 root     disk       10, 237 Jan  1 00:00 loop-control
brw-rw----    1 root     disk        7,   0 Jan  1 00:00 loop0
brw-rw----    1 root     disk        7,   1 Jan  1 00:00 loop1
brw-rw----    1 root     disk        7,   2 Jan  1 00:00 loop2
brw-rw----    1 root     disk        7,   3 Jan  1 00:00 loop3
brw-rw----    1 root     disk        7,   4 Jan  1 00:00 loop4
brw-rw----    1 root     disk        7,   5 Jan  1 00:00 loop5
brw-rw----    1 root     disk        7,   6 Jan  1 00:00 loop6
brw-rw----    1 root     disk        7,   7 Jan  1 00:00 loop7
crw-r-----    1 root     kmem        1,   1 Jan  1 00:00 mem
crw-------    1 root     root       10,  59 Jan  1 00:00 memory_bandwidth
brw-rw----    1 root     disk      179,   0 Jan  1 00:00 mmcblk0
brw-rw----    1 root     disk      179,   1 Jan  1 00:00 mmcblk0p1
brw-rw----    1 root     disk      179,   2 Jan  1 00:00 mmcblk0p2
brw-rw----    1 root     disk      179,   3 Jan  1 00:00 mmcblk0p3
brw-rw----    1 root     disk      179,   8 Jan  1 00:00 mmcblk1
brw-rw----    1 root     disk      179,  16 Jan  1 00:00 mmcblk1boot0
brw-rw----    1 root     disk      179,  24 Jan  1 00:00 mmcblk1boot1
brw-rw----    1 root     disk      179,   9 Jan  1 00:00 mmcblk1p1
brw-rw----    1 root     disk      179,  10 Jan  1 00:00 mmcblk1p2
brw-rw----    1 root     disk      179,  32 Jan  1 00:00 mmcblk1rpmb
crw-------    1 root     root       10,  63 Jan  1 00:00 mxc_asrc
crw-------    1 root     root       10,  61 Jan  1 00:00 network_latency
crw-------    1 root     root       10,  60 Jan  1 00:00 network_throughput
crw-rw-rw-    1 root     root        1,   3 Jan  1 00:00 null
crw-------    1 root     root      108,   0 Jan  1 00:00 ppp
crw-------    1 root     root      252,   0 Jan  1 00:00 pps0
crw-------    1 root     root      252,   1 Jan  1 00:00 pps1
crw-rw-rw-    1 root     tty         5,   2 Jan  1 00:00 ptmx
crw-------    1 root     root      251,   0 Jan  1 00:00 ptp0
crw-------    1 root     root      251,   1 Jan  1 00:00 ptp1
drwxr-xr-x    2 root     root             0 Jan  1 00:00 pts
crw-------    1 root     root       10,  58 Jan  1 00:00 pxp_device
brw-rw----    1 root     disk        1,   0 Jan  1 00:00 ram0
brw-rw----    1 root     disk        1,   1 Jan  1 00:00 ram1
brw-rw----    1 root     disk        1,  10 Jan  1 00:00 ram10
brw-rw----    1 root     disk        1,  11 Jan  1 00:00 ram11
brw-rw----    1 root     disk        1,  12 Jan  1 00:00 ram12
brw-rw----    1 root     disk        1,  13 Jan  1 00:00 ram13
brw-rw----    1 root     disk        1,  14 Jan  1 00:00 ram14
brw-rw----    1 root     disk        1,  15 Jan  1 00:00 ram15
brw-rw----    1 root     disk        1,   2 Jan  1 00:00 ram2
brw-rw----    1 root     disk        1,   3 Jan  1 00:00 ram3
brw-rw----    1 root     disk        1,   4 Jan  1 00:00 ram4
brw-rw----    1 root     disk        1,   5 Jan  1 00:00 ram5
brw-rw----    1 root     disk        1,   6 Jan  1 00:00 ram6
brw-rw----    1 root     disk        1,   7 Jan  1 00:00 ram7
brw-rw----    1 root     disk        1,   8 Jan  1 00:00 ram8
brw-rw----    1 root     disk        1,   9 Jan  1 00:00 ram9
crw-rw-rw-    1 root     root        1,   8 Jan  1 00:00 random
lrwxrwxrwx    1 root     root             4 Jan  1 00:00 rtc -> rtc0
crw-------    1 root     root      253,   0 Jan  1 00:00 rtc0
drwxrwxrwx    2 root     root            40 Jan  1 00:00 shm
drwxr-xr-x    3 root     root           180 Jan  1 00:00 snd
crw-------    1 root     root      242,   0 Jan  1 00:00 spidevx
lrwxrwxrwx    1 root     root            15 Jan  1 00:00 stderr -> /proc/self/fd/2
lrwxrwxrwx    1 root     root            15 Jan  1 00:00 stdin -> /proc/self/fd/0
lrwxrwxrwx    1 root     root            15 Jan  1 00:00 stdout -> /proc/self/fd/1
crw-rw-rw-    1 root     tty         5,   0 Jan  1 00:00 tty
crw--w----    1 root     tty         4,   0 Jan  1 00:00 tty0
crw--w----    1 root     tty         4,   1 Jan  1 00:00 tty1
crw--w----    1 root     tty         4,  10 Jan  1 00:00 tty10
crw--w----    1 root     tty         4,  11 Jan  1 00:00 tty11
crw--w----    1 root     tty         4,  12 Jan  1 00:00 tty12
crw--w----    1 root     tty         4,  13 Jan  1 00:00 tty13
crw--w----    1 root     tty         4,  14 Jan  1 00:00 tty14
crw--w----    1 root     tty         4,  15 Jan  1 00:00 tty15
crw--w----    1 root     tty         4,  16 Jan  1 00:00 tty16
crw--w----    1 root     tty         4,  17 Jan  1 00:00 tty17
crw--w----    1 root     tty         4,  18 Jan  1 00:00 tty18
crw--w----    1 root     tty         4,  19 Jan  1 00:00 tty19
crw--w----    1 root     tty         4,   2 Jan  1 00:00 tty2
crw--w----    1 root     tty         4,  20 Jan  1 00:00 tty20
crw--w----    1 root     tty         4,  21 Jan  1 00:00 tty21
crw--w----    1 root     tty         4,  22 Jan  1 00:00 tty22
crw--w----    1 root     tty         4,  23 Jan  1 00:00 tty23
crw--w----    1 root     tty         4,  24 Jan  1 00:00 tty24
crw--w----    1 root     tty         4,  25 Jan  1 00:00 tty25
crw--w----    1 root     tty         4,  26 Jan  1 00:00 tty26
crw--w----    1 root     tty         4,  27 Jan  1 00:00 tty27
crw--w----    1 root     tty         4,  28 Jan  1 00:00 tty28
crw--w----    1 root     tty         4,  29 Jan  1 00:00 tty29
crw--w----    1 root     tty         4,   3 Jan  1 00:00 tty3
crw--w----    1 root     tty         4,  30 Jan  1 00:00 tty30
crw--w----    1 root     tty         4,  31 Jan  1 00:00 tty31
crw--w----    1 root     tty         4,  32 Jan  1 00:00 tty32
crw--w----    1 root     tty         4,  33 Jan  1 00:00 tty33
crw--w----    1 root     tty         4,  34 Jan  1 00:00 tty34
crw--w----    1 root     tty         4,  35 Jan  1 00:00 tty35
crw--w----    1 root     tty         4,  36 Jan  1 00:00 tty36
crw--w----    1 root     tty         4,  37 Jan  1 00:00 tty37
crw--w----    1 root     tty         4,  38 Jan  1 00:00 tty38
crw--w----    1 root     tty         4,  39 Jan  1 00:00 tty39
crw--w----    1 root     tty         4,   4 Jan  1 00:00 tty4
crw--w----    1 root     tty         4,  40 Jan  1 00:00 tty40
crw--w----    1 root     tty         4,  41 Jan  1 00:00 tty41
crw--w----    1 root     tty         4,  42 Jan  1 00:00 tty42
crw--w----    1 root     tty         4,  43 Jan  1 00:00 tty43
crw--w----    1 root     tty         4,  44 Jan  1 00:00 tty44
crw--w----    1 root     tty         4,  45 Jan  1 00:00 tty45
crw--w----    1 root     tty         4,  46 Jan  1 00:00 tty46
crw--w----    1 root     tty         4,  47 Jan  1 00:00 tty47
crw--w----    1 root     tty         4,  48 Jan  1 00:00 tty48
crw--w----    1 root     tty         4,  49 Jan  1 00:00 tty49
crw--w----    1 root     tty         4,   5 Jan  1 00:00 tty5
crw--w----    1 root     tty         4,  50 Jan  1 00:00 tty50
crw--w----    1 root     tty         4,  51 Jan  1 00:00 tty51
crw--w----    1 root     tty         4,  52 Jan  1 00:00 tty52
crw--w----    1 root     tty         4,  53 Jan  1 00:00 tty53
crw--w----    1 root     tty         4,  54 Jan  1 00:00 tty54
crw--w----    1 root     tty         4,  55 Jan  1 00:00 tty55
crw--w----    1 root     tty         4,  56 Jan  1 00:00 tty56
crw--w----    1 root     tty         4,  57 Jan  1 00:00 tty57
crw--w----    1 root     tty         4,  58 Jan  1 00:00 tty58
crw--w----    1 root     tty         4,  59 Jan  1 00:00 tty59
crw--w----    1 root     tty         4,   6 Jan  1 00:00 tty6
crw--w----    1 root     tty         4,  60 Jan  1 00:00 tty60
crw--w----    1 root     tty         4,  61 Jan  1 00:00 tty61
crw--w----    1 root     tty         4,  62 Jan  1 00:00 tty62
crw--w----    1 root     tty         4,  63 Jan  1 00:00 tty63
crw--w----    1 root     tty         4,   7 Jan  1 00:00 tty7
crw--w----    1 root     tty         4,   8 Jan  1 00:00 tty8
crw--w----    1 root     tty         4,   9 Jan  1 00:00 tty9
crw-------    1 root     root      207,  16 Jan  1 00:04 ttymxc0  //nisy:uart0接口
crw-rw----    1 root     dialout   207,  18 Jan  1 00:00 ttymxc2  //nisy:uart2接口
crw-rw----    1 root     dialout   207,  21 Jan  1 00:00 ttymxc5   //nisy:uart5接口
crw-------    1 root     root       10,  57 Jan  1 00:00 ubi_ctrl
crw-rw-rw-    1 root     root        1,   9 Jan  1 00:00 urandom
drwxr-xr-x    3 root     root            60 Jan  1 00:00 v4l
crw-rw----    1 root     tty         7,   0 Jan  1 00:00 vcs
crw-rw----    1 root     tty         7,   1 Jan  1 00:00 vcs1
crw-rw----    1 root     tty         7, 128 Jan  1 00:00 vcsa
crw-rw----    1 root     tty         7, 129 Jan  1 00:00 vcsa1
crw-rw----    1 root     video      81,   0 Jan  1 00:00 video0
crw-------    1 root     root       10, 130 Jan  1 00:00 watchdog
crw-------    1 root     root      248,   0 Jan  1 00:00 watchdog0
crw-rw-rw-    1 root     root        1,   5 Jan  1 00:00 zero
[root@imx6ull:/dev]#
```

### procfs
#### [root\@imx6ull:/proc]# cat /etc/os-release
```
    NAME=Buildroot
    VERSION=2019.02-g4946908395-dirty
    ID=buildroot
    VERSION_ID=2019.02
    PRETTY_NAME="Buildroot 2019.02"
```

#### \[root\@imx6ull:/proc]# cat cpuinfo

    processor       : 0
    model name      : ARMv7 Processor rev 5 (v7l)
    BogoMIPS        : 3.00
    Features        : half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae
    CPU implementer : 0x41
    CPU architecture: 7
    CPU variant     : 0x0
    CPU part        : 0xc07
    CPU revision    : 5

    Hardware        : Freescale i.MX6 UltraLite (Device Tree)
    Revision        : 0000
    Serial          : 0000000000000000

#### \[root\@imx6ull:/proc]# cat meminfo

    MemTotal:         503392 kB
    MemFree:          309756 kB
    MemAvailable:     376664 kB
    Buffers:            3752 kB
    Cached:            67880 kB
    SwapCached:            0 kB
    Active:            44200 kB
    Inactive:          53348 kB
    Active(anon):      26692 kB
    Inactive(anon):     1076 kB
    Active(file):      17508 kB
    Inactive(file):    52272 kB
    Unevictable:           0 kB
    Mlocked:               0 kB
    HighTotal:             0 kB
    HighFree:              0 kB
    LowTotal:         503392 kB
    LowFree:          309756 kB
    SwapTotal:             0 kB
    SwapFree:              0 kB
    Dirty:                 8 kB
    Writeback:             0 kB
    AnonPages:         25952 kB
    Mapped:            37540 kB
    Shmem:              1852 kB
    Slab:              12124 kB
    SReclaimable:       3932 kB
    SUnreclaim:         8192 kB
    KernelStack:         848 kB
    PageTables:          920 kB
    NFS_Unstable:          0 kB
    Bounce:                0 kB
    WritebackTmp:          0 kB
    CommitLimit:      251696 kB
    Committed_AS:      39004 kB
    VmallocTotal:    1556480 kB
    VmallocUsed:           0 kB
    VmallocChunk:          0 kB
    CmaTotal:         327680 kB   
    CmaFree:          252872 kB

#### \[root\@imx6ull:/proc]# cat mounts

    /dev/root / ext4 rw,relatime,data=ordered 0 0
    devtmpfs /dev devtmpfs rw,relatime,size=87344k,nr_inodes=21836,mode=755 0 0
    proc /proc proc rw,relatime 0 0
    devpts /dev/pts devpts rw,relatime,gid=5,mode=620,ptmxmode=666 0 0
    tmpfs /dev/shm tmpfs rw,relatime,mode=777 0 0
    tmpfs /tmp tmpfs rw,relatime 0 0
    tmpfs /run tmpfs rw,nosuid,nodev,relatime,mode=755 0 0
    sysfs /sys sysfs rw,relatime 0 0
    debugfs /sys/kernel/debug debugfs rw,relatime 0 0
    [root@imx6ull:/proc]#


#### \[root\@imx6ull:/proc]# cat cmdline

    console=ttymxc0,115200 root=/dev/mmcblk1p2 rootwait rw 

#### \[root\@imx6ull:/proc]# cat interrupts

               CPU0
     16:       9289       GPC  55 Level     i.MX Timer Tick
     19:         22       GPC  33 Level     2010000.ecspi
     20:       9050       GPC  26 Level     2020000.serial
     21:          0       GPC  98 Level     sai
     22:          0       GPC  50 Level     2034000.asrc
     40:          0       GPC   4 Level     20cc000.snvs:snvs-powerkey
     41:       2884       GPC 120 Level     20b4000.ethernet
     42:          0       GPC 121 Level     20b4000.ethernet
     43:          0       GPC  80 Level     20bc000.wdog
     44:          0       GPC  49 Level     imx_thermal
     49:          0       GPC  19 Level     rtc alarm
     55:          0       GPC   2 Level     sdma
     60:          1       GPC  43 Level     2184000.usb
     61:       1551       GPC  42 Level     2184200.usb
     62:          0       GPC 118 Level     2188000.ethernet
     63:          0       GPC 119 Level     2188000.ethernet
     64:          0       GPC  22 Level     mmc0
     65:       4778       GPC  23 Level     mmc1
     66:          1       GPC 100 Level     2198000.adc
     67:          0       GPC  36 Level     21a0000.i2c
     68:        291       GPC  37 Level     21a4000.i2c
     70:          3       GPC   5 Level     21c8000.lcdif
     71:          0       GPC   8 Level     pxp-dmaengine-legacy
     72:          0       GPC  18 Level     pxp-dmaengine-std
     73:          0       GPC  28 Level     21ec000.serial
     74:          0       GPC  17 Level     21fc000.serial
     75:          0       GPC  46 Level     dcp-vmi-irq
     76:          0       GPC  47 Level     dcp-irq
     78:          2       GPC   6 Level     imx-rng
     80:          0  gpio-mxc   1 Edge      inv_mpu
     97:          0  gpio-mxc  18 Edge      SII902x_det
     98:          0  gpio-mxc  19 Edge      2190000.usdhc cd
    189:          0  gpio-mxc  14 Edge      User2 Button
    208:          0  gpio-mxc   1 Edge      User1 Button
    IPI0:          0  CPU wakeup interrupts
    IPI1:          0  Timer broadcast interrupts
    IPI2:          0  Rescheduling interrupts
    IPI3:          0  Function call interrupts
    IPI4:          0  CPU stop interrupts
    IPI5:       3858  IRQ work interrupts
    IPI6:          0  completion interrupts
    Err:          0

#### \[root\@imx6ull:/proc]# cat devices

    [  400.697322] random: crng init done

    Character devices:
      1 mem
      4 /dev/vc/0
      4 tty
      5 /dev/tty
      5 /dev/console
      5 /dev/ptmx
      7 vcs
     10 misc
     13 input
     29 fb
     81 video4linux
     89 i2c
     90 mtd
    108 ppp
    116 alsa
    128 ptm
    136 pts
    166 ttyACM
    180 usb
    188 ttyUSB
    189 usb_device
    207 ttymxc
    216 rfcomm
    226 drm
    241 adxl345
    242 spidevx
    243 irda
    244 dht11
    245 ds18b20
    246 ttyGS
    247 ttyLP
    248 watchdog
    249 tee
    250 iio
    251 ptp
    252 pps
    253 rtc
    254 gpiochip

    Block devices:
      1 ramdisk
    259 blkext
      7 loop
      8 sd
     31 mtdblock
     65 sd
     66 sd
     67 sd
     68 sd
     69 sd
     70 sd
     71 sd
    128 sd
    129 sd
    130 sd
    131 sd
    132 sd
    133 sd
    134 sd
    135 sd
    179 mmc
    [root@imx6ull:/proc]#
    [root@imx6ull:/proc]#

#### \[root\@imx6ull:/proc]# cat uptime

    411.42 393.86

#### \[root\@imx6ull:/proc]# cat iomem

    00905000-0091ffff : 905000.sram
    01804000-01805fff : /soc/dma-apbh@01804000
    02010000-02013fff : /soc/aips-bus@02000000/spba-bus@02000000/ecspi@02010000
    02020000-02023fff : /soc/aips-bus@02000000/spba-bus@02000000/serial@02020000
    0202c000-0202ffff : /soc/aips-bus@02000000/spba-bus@02000000/sai@0202c000
    02034000-02037fff : /soc/aips-bus@02000000/spba-bus@02000000/asrc@02034000
    02080000-02083fff : /soc/aips-bus@02000000/pwm@02080000
    02084000-02087fff : /soc/aips-bus@02000000/pwm@02084000
    02088000-0208bfff : /soc/aips-bus@02000000/pwm@02088000
    0208c000-0208ffff : /soc/aips-bus@02000000/pwm@0208c000
    02090000-02093fff : /soc/aips-bus@02000000/can@02090000
    0209c000-0209ffff : /soc/aips-bus@02000000/gpio@0209c000
    020a0000-020a3fff : /soc/aips-bus@02000000/gpio@020a0000
    020a4000-020a7fff : /soc/aips-bus@02000000/gpio@020a4000
    020a8000-020abfff : /soc/aips-bus@02000000/gpio@020a8000
    020ac000-020affff : /soc/aips-bus@02000000/gpio@020ac000
    020b4000-020b7fff : /soc/aips-bus@02000000/ethernet@020b4000
    020bc000-020bffff : /soc/aips-bus@02000000/wdog@020bc000
    020c9000-020c9fff : /soc/aips-bus@02000000/usbphy@020c9000
    020ca000-020cafff : /soc/aips-bus@02000000/usbphy@020ca000
    020e0000-020e3fff : /soc/aips-bus@02000000/iomuxc@020e0000
    020ec000-020effff : /soc/aips-bus@02000000/sdma@020ec000
    020f0000-020f3fff : /soc/aips-bus@02000000/pwm@020f0000
    020f4000-020f7fff : /soc/aips-bus@02000000/pwm@020f4000
    020f8000-020fbfff : /soc/aips-bus@02000000/pwm@020f8000
    020fc000-020fffff : /soc/aips-bus@02000000/pwm@020fc000
    02184000-021841ff : /soc/aips-bus@02100000/usb@02184000
      02184000-021841ff : /soc/aips-bus@02100000/usb@02184000
    02184200-021843ff : /soc/aips-bus@02100000/usb@02184200
      02184200-021843ff : /soc/aips-bus@02100000/usb@02184200
    02184800-021849ff : /soc/aips-bus@02100000/usbmisc@02184800
    02188000-0218bfff : /soc/aips-bus@02100000/ethernet@02188000
    02190000-02193fff : /soc/aips-bus@02100000/usdhc@02190000
    02194000-02197fff : /soc/aips-bus@02100000/usdhc@02194000
    02198000-0219bfff : /soc/aips-bus@02100000/adc@02198000
    021a0000-021a3fff : /soc/aips-bus@02100000/i2c@021a0000
    021a4000-021a7fff : /soc/aips-bus@02100000/i2c@021a4000
    021b8000-021bbfff : /soc/aips-bus@02100000/weim@021b8000
    021bc000-021bffff : /soc/aips-bus@02100000/ocotp-ctrl@021bc000
    021c8000-021cbfff : /soc/aips-bus@02100000/lcdif@021c8000
    021cc000-021cffff : /soc/aips-bus@02100000/pxp@021cc000
    021ec000-021effff : /soc/aips-bus@02100000/serial@021ec000
    021fc000-021fffff : /soc/aips-bus@02100000/serial@021fc000
    02280000-02283fff : /soc/aips-bus@02200000/dcp@02280000
    02290000-0229ffff : /soc/aips-bus@02200000/iomuxc-snvs@02290000
    80000000-9fffffff : System RAM   =512MB
      80008000-80efffff : Kernel code
      81000000-8112aa5f : Kernel data
    [root@imx6ull:/proc]#  

### sysfs
#### [root@imx6ull:~]# ls -al /sys/devices/platform/
```
total 0
drwxr-xr-x   10 root     root             0 Jan  1 00:00 .
drwxr-xr-x    9 root     root             0 Jan  1 00:00 ..
drwxr-xr-x    4 root     root             0 Jan  1 00:00 Fixed MDIO bus.0
drwxr-xr-x    4 root     root             0 Jan  1 00:00 Vivante GCCore
drwxr-xr-x    3 root     root             0 Jan  1 00:00 alarmtimer
drwxr-xr-x    3 root     root             0 Jan  1 00:00 imx6q-cpufreq
drwxr-xr-x    2 root     root             0 Jan  1 00:02 power
drwxr-xr-x    4 root     root             0 Jan  1 00:00 reg-dummy
drwxr-xr-x    3 root     root             0 Jan  1 00:00 regulatory.0
drwxr-xr-x    3 root     root             0 Jan  1 00:00 snd-soc-dummy
-rw-r--r--    1 root     root          4096 Jan  1 00:00 uevent
[root@imx6ull:~]#
```

#### 查询温度

```
[root@100ask:~]# for i in /sys/devices/virtual/thermal/thermal_zone* ; do echo $i ;echo type:$(cat $i/type); echo temp:$(cat $i/temp); echo trip_point_0_type:$(cat $i/trip_point_0_type); echo trip_point_0_hyst:$(cat $i/trip_point_0_hyst); echo trip_point_0_temp:$(cat $i/trip_point_0_temp); echo trip_point_1_type:$(cat $i/trip_point_1_type); echo trip_point_1_hyst:$(cat $i/trip_point_1_hyst); echo trip_point_1_temp:$(cat $i/trip_point_1_temp);echo; done

/sys/devices/virtual/thermal/thermal_zone0
type:imx_thermal_zone
temp:47831
trip_point_0_type:passive
cat: /sys/devices/virtual/thermal/thermal_zone0/trip_point_0_hyst: No such file or directory
trip_point_0_hyst:
trip_point_0_temp:95000
trip_point_1_type:critical
cat: /sys/devices/virtual/thermal/thermal_zone0/trip_point_1_hyst: No such file or directory
trip_point_1_hyst:
trip_point_1_temp:100000

[root@100ask:~]#

```


