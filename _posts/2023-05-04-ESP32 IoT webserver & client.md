---
layout: post
title: IoT OS
author: [Richard Kuo]
category: [Lecture]
tags: [jekyll, ai]
---

Introduction to IoT Operating Systems

---
## IoT Operating Systems
[9 IoT Operating Systems To Use in 2021 [List & Comparison]](https://ubidots.com/blog/iot-operating-systems/)
* [Contiki](https://www.contiki-ng.org/)
* [freeRTOS](https://www.freertos.org/)
* [Mbed OS](https://os.mbed.com/mbed-os/)
* [MicroPython](https://micropython.org/)
* [Embedded Linux](https://ubuntu.com/embedded)
* [RIOT](https://www.riot-os.org/)
* [Tiny OS](http://www.tinyos.net/)
* [Windows10 IoT](https://developer.microsoft.com/en-us/windows/iot/)
* [OpenWrt](https://openwrt.org/)
![](https://github.com/rkuo2000/MCU-course/blob/main/images/IoT-OS_use_cases.png?raw=true)

---
### [freeRTOS](https://www.freertos.org/)
![](https://www.freertos.org/fr-content-src/uploads/2018/07/logo-1.jpg)
Real-time operating system for microcontrollers

[AWS IoT FreeRTOS使用者指南](https://docs.aws.amazon.com/zh_tw/freertos/archive/202012.00/userguide/what-is-freertos.html)<br>
**FreeRTOS架構**<br>
![](https://docs.aws.amazon.com/zh_tw/freertos/archive/202012.00/userguide/images/afreertos-architecture.png)

---
### Arduino Library: TridentTD_EasyFreeRTOS32
![](https://github.com/rkuo2000/MCU-course/blob/main/images/Arduino_Library_TridentTD_EasyFreeRTOS32.png?raw=true)

### examples> TridentTD_EasyFreeRTOS32 >01.basic_multitask
[01.basic_multitask](https://github.com/TridentTD/TridentTD_EasyFreeRTOS32/tree/master/example/01.basic_multitask)

### examples> TridentTD_EasyFreeRTOS32 >01.basic_multitask
[02.advance_multitask](https://github.com/TridentTD/TridentTD_EasyFreeRTOS32/tree/master/example/02.advance_multitask)

---
### [RIOT](https://www.riot-os.org/)
![](https://github.com/RIOT-OS/RIOT/raw/master/doc/doxygen/src/riot-logo.svg)
RIOT is a real-time multi-threading operating system that supports a range of devices that are typically found in the Internet of Things (IoT): 8-bit, 16-bit and 32-bit microcontrollers.
![](https://github.com/rkuo2000/MCU-course/blob/main/images/RIOT_introduction.png?raw=true)
### [RIOT Code](https://github.com/RIOT-OS/RIOT)
[RIOT Documents](https://doc.riot-os.org/)
![](https://github.com/rkuo2000/MCU-course/blob/main/images/RIOT_structure.png?raw=true)

---
### [Supported Boards](https://www.riot-os.org/boards.html)
[RIOT-OS on ESP32 boards](https://doc.riot-os.org/group__cpu__esp32.html)<br>

---
### Toolchain Installation
**[Standard Setup of Toolchain for Windows](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/windows-setup.html)**<br>
1. run [ESP-IDF Installer (556.65MB)](https://dl.espressif.com/dl/esp-idf-tools-setup-2.4.exe)
2. run ESP-IDF Command Prompt / PowerShell
3. run batch
 - `cd C:/Users/USER/Desktop/esp-idf>`
 - `install.bat`
 - `export.bat`
 
**Install ESP-IDF on Ubuntu/Linux OS**<br>
```
mkdir -p ~/esp
cd ~/esp
git clone --recursive https://github.com/espressif/esp-idf
cd esp-idf
./install.sh esp32
. ./export.sh
```
* Setup ESP32 toolchain environment
```
cd ~/esp/esp-idf
. ./export.sh
```

---
### Download RIOT
```
cd ~
git clone https://github.com/RIOT-OS/RIOT
```

---
### ESP-IDF freeRTOS example
[freeRTOS](https://github.com/espressif/esp-idf/tree/master/examples/system/freertos/real_time_stats)<br>
[real_time_stats_example_main.c](https://github.com/espressif/esp-idf/blob/master/examples/system/freertos/real_time_stats/main/real_time_stats_example_main.c)<br>
```
cd ~/esp/esp-idf/examples/system/freertos/real_time_stats
idf.py build
idf.py flash -p /dev/ttyUSB0
```
![](https://github.com/rkuo2000/MCU-course/blob/main/images/freeRTOS_real_time_stats_monitor.png?raw=true)

---
### RIOT Examples
[examples](https://github.com/RIOT-OS/RIOT/tree/master/examples)<br>
[tests](https://github.com/RIOT-OS/RIOT/tree/master/tests)<br>
`cd ~/RIOT`<br>

* [arduino_hello-world](https://github.com/RIOT-OS/RIOT/tree/master/examples/arduino_hello-world)<br>
`make flash BOARD=esp32-wroom-32 -C examples/arduino_hello-world /dev/ttyUSB0`<br>
![](https://github.com/rkuo2000/MCU-course/blob/main/images/RIOT_examples_arduino_hello-world_compilation.png?raw=true)
![](https://github.com/rkuo2000/MCU-course/blob/main/images/RIOT_examples_arduino_hello-world_monitor.png?raw=true)
* [blinky](https://github.com/RIOT-OS/RIOT/tree/master/examples/blinky)<br>
`make flash BOARD=esp32-wroom-32 -C examples/blinky /dev/ttyUSB0`<br>
![](https://github.com/rkuo2000/MCU-course/blob/main/images/RIOT_examples_blinky_compilation.png?raw=true)
![](https://github.com/rkuo2000/MCU-course/blob/main/images/RIOT_examples_blinky_monitor.png?raw=true)
* [thread_basic](https://github.com/RIOT-OS/RIOT/tree/master/tests/thread_basic)<br>
`make flash BOARD=esp32-wroom-32 -C tests/thread_basic /dev/ttyUSB0`<br>
![](https://github.com/rkuo2000/MCU-course/blob/main/images/RIOT_tests_thread_basic_compilation.png?raw=true)
![](https://github.com/rkuo2000/MCU-course/blob/main/images/RIOT_tests_thread_basic_monitor.png?raw=true)

<br>
<br>

*This site was last updated {{ site.time | date: "%B %d, %Y" }}.*

