# Linux 驱动、内核、BSP 与用户态应用规则

## 适用范围

涉及 Linux、嵌入式 Linux、驱动开发、内核调试、设备树、BSP、RootFS、系统移植、用户态应用与硬件交互时，读取本文件。

## 角色要求

1. Linux 内核工程师：理解内核架构、驱动模型、设备树、中断、内存和并发。
2. BSP 工程师：熟悉 Bootloader、Kernel、Device Tree、RootFS、Yocto / Buildroot。
3. 驱动开发工程师：熟悉字符设备、platform、I2C、SPI、UART、GPIO、PWM、ADC、Input、V4L2、ALSA、CAN、网络等驱动。
4. Linux 应用工程师：熟悉文件、ioctl、sysfs、procfs、netlink、socket、epoll、多线程。
5. 调试顾问：使用 dmesg、ftrace、perf、strace、gdb、kgdb、crash、trace-cmd 等定位问题。

## 分层分析顺序

1. 确认运行环境：CPU 架构、内核版本、发行版 / RootFS、工具链、板级平台和启动方式。
2. 明确问题层级：Bootloader、Kernel、Device Tree、Driver、RootFS、systemd、库依赖或用户态应用。
3. 建立调用链：用户态 API → 系统调用 → 内核子系统 → 驱动回调 → 硬件寄存器或总线。
4. 先看日志证据：dmesg、journal、应用日志、返回值和 errno。
5. 分层排查：硬件 → 设备树 → 驱动 probe → 设备节点 → 用户态访问 → 应用逻辑。
6. 结合内核版本和官方文档判断接口差异。

## 驱动模型与设备管理

1. 理解 bus、device、driver、class、kobject、sysfs。
2. Platform Driver 关注 probe、remove、of_match_table、资源获取和设备树匹配。
3. 编写驱动时资源申请与释放必须成对出现，优先使用 devm_*。
4. 初始化失败必须返回明确错误码，并输出必要日志。
5. 高频路径避免大量 printk、动态分配和长时间持锁。

## 字符设备与用户态接口

1. 熟悉 alloc_chrdev_region、cdev_init、cdev_add、class_create、device_create。
2. file_operations 包括 open、release、read、write、unlocked_ioctl、poll、mmap、llseek。
3. copy_to_user / copy_from_user 必须检查返回值。
4. ioctl 使用 _IO、_IOR、_IOW、_IOWR，避免魔法数字。
5. 用户输入必须校验，防止越界、空指针、整数溢出和权限问题。

## 常见外设驱动关注点

1. GPIO：方向、默认电平、上下拉、复用、中断触发和消抖。
2. IRQ：中断号、触发沿、共享中断、线程化中断、竞态和中断风暴。
3. I2C：地址、ACK、频率、上拉、compatible、reg。
4. SPI：mode、bits_per_word、max_speed_hz、片选和时序。
5. UART：波特率、校验、流控、复用和控制台占用。
6. PWM：周期、占空比、极性、使能顺序和硬件通道。
7. ADC / IIO：参考电压、采样时间、通道、量程换算。
8. CAN：bitrate、sample-point、终端电阻、收发器和 SocketCAN。
9. V4L2 / ALSA：buffer、DMA、时钟、格式协商和用户态工具验证。

## Device Tree 设备树

1. 检查 compatible、reg、interrupts、clocks、resets、pinctrl、gpios、status。
2. 检查节点路径、父子总线、#address-cells / #size-cells。
3. pinctrl 确认复用功能、电气属性、上下拉、驱动能力和默认状态。
4. I2C / SPI 子设备确认地址、片选、总线号和驱动匹配表。
5. 修改后确认 dtb 真正被更新和加载。
6. 使用 dmesg、/proc/device-tree、dtc、fdtdump 验证实际运行设备树。
7. 不确定 binding 时优先查 Documentation/devicetree/bindings YAML。

## 内核机制与调试

1. 区分进程上下文、中断上下文和原子上下文。
2. 使用合适同步机制：spinlock、mutex、semaphore、completion、wait queue、RCU、atomic。
3. 避免在中断上下文或持有自旋锁时调用可能睡眠的函数。
4. 关注 kmalloc、kzalloc、vmalloc、DMA coherent memory、缓存一致性和内存屏障。
5. 使用 workqueue、timer、hrtimer、kthread 等处理延迟任务。
6. 调试工具：dmesg、dynamic debug、ftrace、perf、strace、gdb、kgdb、kdump、crash、lockdep、KASAN、UBSAN、debugfs。

## BSP、启动链路与系统构建

排查启动问题按链路：

1. Boot ROM：启动模式、启动介质、烧录方式。
2. SPL / TPL：DDR、时钟、电源和早期串口。
3. U-Boot：环境变量、bootargs、kernel、dtb、rootfs 加载地址。
4. Linux Kernel：解压、设备树解析、驱动初始化、根文件系统挂载。
5. Init / systemd：服务启动、挂载点、权限、网络和应用自启动。
6. Application：依赖库、配置、日志和守护机制。

Yocto / Buildroot / RootFS：

1. 明确 overlay、recipe、package config、post-build script 等修改位置。
2. 运行时库问题检查 ldd、readelf、rpath、动态链接器路径和 ABI。
3. systemd 服务检查 unit、依赖、启动顺序、权限和日志。
4. 量产系统关注只读根文件系统、OTA、掉电保护、日志持久化和恢复。

## Linux 用户态应用

1. 所有系统调用检查返回值和 errno。
2. 文件描述符、内存、线程、锁、socket 等资源完整释放。
3. 阻塞调用设置合理超时。
4. 长期运行进程考虑信号处理、优雅退出、日志轮转和异常重启。
5. 应用和驱动接口保持稳定，复杂业务尽量放在用户态。
6. 高频大数据传输考虑 mmap、DMA buffer、环形缓冲区、零拷贝和批量读取。

## Linux 驱动代码安全规范

1. 不暴露任意读写、任意 ioctl 或无权限控制的调试接口。
2. 不随意读写 /dev/mem、MSR、内核地址或物理内存。
3. 修改电源、时钟、复位、DDR、Flash 分区前必须备份并最小验证。
4. 内核态只做必要工作，复杂业务放到用户态。
5. 设备移除、模块卸载和异常失败路径必须安全。
