# 常用命令模板

## 通用项目命令

```bash
# Maven 项目
mvn clean compile
mvn test
mvn spring-boot:run

# Node.js 项目
npm install
npm test
npm start

# Python 项目
pip install -r requirements.txt
pytest
python manage.py runserver
```

## 嵌入式常用命令

```bash
# PlatformIO
pio run
pio run -t upload
pio device monitor

# ESP-IDF
idf.py build
idf.py flash
idf.py monitor

# Zephyr
west build -b <board>
west flash
west debug

# CMake 嵌入式项目
cmake -S . -B build
cmake --build build

# OpenOCD 调试
openocd -f interface/stlink.cfg -f target/stm32f4x.cfg

# 串口查看
screen /dev/ttyUSB0 115200
minicom -D /dev/ttyUSB0 -b 115200
```

## Linux 驱动与系统调试命令

```bash
# 内核日志
dmesg -w
dmesg | grep -i <keyword>
journalctl -k -f

# 设备树
ls /proc/device-tree
cat /proc/device-tree/model
dtc -I fs -O dts /proc/device-tree > running.dts

# 设备和驱动
ls /sys/bus/platform/devices
ls /sys/bus/platform/drivers
ls /sys/class
cat /proc/devices
ls -l /dev

# 模块
lsmod
modinfo <module_name>
insmod <module.ko>
rmmod <module_name>
modprobe <module_name>

# 中断
cat /proc/interrupts
watch -n 1 cat /proc/interrupts

# GPIO
gpioinfo
gpioget gpiochip0 <line>
gpioset gpiochip0 <line>=1

# I2C
i2cdetect -y <bus>
i2cdump -y <bus> <addr>
i2cget -y <bus> <addr> <reg>
i2cset -y <bus> <addr> <reg> <value>

# SPI
spidev_test -D /dev/spidevX.Y

# CAN
ip link set can0 up type can bitrate 500000
candump can0
cansend can0 123#11223344

# 串口
stty -F /dev/ttyS0 115200
cat /dev/ttyS0
echo test > /dev/ttyS0

# 用户态程序调试
strace ./app
ltrace ./app
gdb ./app
valgrind ./app

# 性能分析
top
htop
perf top
perf record ./app
perf report
```

## 最小验证闭环

1. 先确认环境能构建。
2. 再运行最小测试。
3. 然后验证关键模块。
4. 最后集成完整功能。
5. 硬件问题先单点验证，再系统联调。
6. Linux 驱动问题先验证设备树和 probe，再验证用户态访问。
