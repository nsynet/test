[toc]





# 奔跑吧入门篇第二版

## 常用命令

### 4.2    实验 4-1：通过 QEMU 调试 ARM64 Linux 内核 

运行 run_rlk_arm64.sh 脚本以启动 QEMU 虚拟机和 GDB 服务。 
``` 
./run_rlk_arm64.sh run debug 
```
在另外一个超级终端中启动 GDB。 
```
$ cd runninglinuxkernel_5.0 
$ gdb-multiarch --tui vmlinux 
(gdb) set architecture aarch64          <= 设置Aarch64架构 
(gdb) target remote localhost:1234     <= 通过1234端口远程连接到QEMU虚拟机 
(gdb) b start_kernel                   <= 在内核的start_kernel处设置断点 
(gdb) c
```
如图 4.5 所示，GDB 开始接管 Linux 内核的运行，并且在断点处暂停，这时即可
使用 GDB 命令来调试内核。

### 4.3    实验 4-2：通过 Eclipse+QEMU 单步调试内核 

在 Eclipse 的 Debugger Console 选项卡（见图 4.12）中输入 
```
file vmlinux 
```
导入调试文件的符号表；输入 
```
set architecture aarch64
```
选择 GDB 支持的 ARM64 架构，如图 4.12 所示。 
在 Debugger Console 选项卡中输入 
```
b _do_fork
```
在_do_fork()函数中设置一个断点。输入 `c` 命令，开始运行 QEMU 虚拟机中的 Linux 内核，程序会停在_do_fork()函数中，如图 4.13 所示。 




# Linux设备驱动开发详解：基于最新的Linux4.0内核_宋宝华


# 2017_Packt.Mastering.Embedded.Linux.Programming.2nd.Edition

# 2019_Linux Device Driver Development Cookbook - Develop Custom Drivers For Your Embedded Linux Applications