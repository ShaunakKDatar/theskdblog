+++
title = "Running RTEMS on Raspberry Pi 4B"
date = "2025-06-11T21:42:22+05:30"
author = "Shaunak Datar"
authorTwitter = "datar_shaunak" # your X handle, if you want it shown
#cover = "images/rtems-rpi4.jpg" # optional: set a relevant image path
coverCaption = "RTEMS booting on a Raspberry Pi 4B" # optional
tags = ["RTEMS", "Raspberry Pi", "Bare-metal", "GSoC"]
keywords = ["RTEMS on Raspberry Pi", "bare-metal RTOS", "GSoC 2025", "Shaunak Datar blog", "rtems raspberry pi tutorial"]
description = "A detailed breakdown of how I got RTEMS running on a Raspberry Pi 4B — from board bring-up to driver development."
showFullContent = false
readingTime = true
hideComments = false
color = "blue" # pick one from your theme palette or leave empty
+++


**RTEMS (Real-Time Executive for Multiprocessor Systems)** is a real-time operating system used in embedded systems, offering a rich set of Board Support Packages (BSPs) for various platforms. One such platform is the **Raspberry Pi 4B**, a popular board among hobbyists, educators, and industry professionals. In this post, I’ll walk you through setting up RTEMS on the Raspberry Pi 4B from scratch.

Let’s dive in!

---

# 🚀 Setting Up the Environment

First, create a working directory. Ensure that `git` is installed on your host machine.

```bash
mkdir -p $HOME/quick-start/src
cd $HOME/quick-start/src
```

Clone the necessary repositories:

```bash
git clone https://gitlab.rtems.org/rtems/tools/rtems-source-builder.git rsb
git clone https://gitlab.rtems.org/rtems/rtos/rtems.git
```

The `rtems` repo contains the RTEMS source, while `rsb` (RTEMS Source Builder) helps you build the toolchain and BSP.

---

# 🔧 Installing the Toolchain

Now, build and install the AArch64 toolchain for the Raspberry Pi 4B:

```bash
cd $HOME/quick-start/src/rsb/rtems
../source-builder/sb-set-builder --prefix=$HOME/quick-start/rtems/7 7/rtems-aarch64
```

Once completed, verify the toolchain:

```bash
$HOME/quick-start/rtems/7/bin/aarch64-rtems7-gcc --version
```

---

# 🧱 Building the BSP

Let’s build the **aarch64/raspberrypi4b** BSP.

```bash
cd $HOME/quick-start/src/rtems
echo "[aarch64/raspberrypi4b]" > config.ini
./waf configure --prefix=$HOME/quick-start/rtems/7
./waf
./waf install
```

---

# 📝 Creating the RTEMS Application

Now that the BSP is ready, let’s write a basic RTEMS application.

```bash
mkdir -p $HOME/quick-start/src/app/hello
cd $HOME/quick-start/src/app/hello
```

Download Waf and make it executable:

```bash
curl https://waf.io/waf-2.0.19 > waf
chmod +x waf
```

Initialize a Git repo and add RTEMS Waf support:

```bash
git init
git submodule add https://gitlab.rtems.org/rtems/tools/rtems_waf.git rtems_waf
```

### 🛠 `wscript`

```python
from __future__ import print_function

rtems_version = "7"

try:
    import rtems_waf.rtems as rtems
except:
    print('error: no rtems_waf git submodule')
    import sys
    sys.exit(1)

def init(ctx):
    rtems.init(ctx, version = rtems_version, long_commands = True)

def bsp_configure(conf, arch_bsp):
    pass

def options(opt):
    rtems.options(opt)

def configure(conf):
    rtems.configure(conf, bsp_configure = bsp_configure)

def build(bld):
    rtems.build(bld)

    bld(features = 'c cprogram',
        target = 'hello.exe',
        cflags = '-g -O2',
        source = 'hello.c')
```

### 💬 `hello.c`

```c
#include <rtems.h>
#include <stdlib.h>
#include <stdio.h>

rtems_task Init(rtems_task_argument ignored)
{
  printf("\nHello World\n");
  printf("I am Shaunak Datar\n");
  exit(0);
}

#define CONFIGURE_APPLICATION_NEEDS_CLOCK_DRIVER
#define CONFIGURE_APPLICATION_NEEDS_CONSOLE_DRIVER
#define CONFIGURE_UNLIMITED_OBJECTS
#define CONFIGURE_UNIFIED_WORK_AREAS
#define CONFIGURE_RTEMS_INIT_TASKS_TABLE
#define CONFIGURE_INIT

#include <rtems/confdefs.h>
```

Configure and build the app:

```bash
./waf configure --rtems=$HOME/quick-start/rtems/7 --rtems-bsp=aarch64/raspberrypi
./waf
```

Create the bootable kernel image:

```bash
~/quick-start/rtems/7/bin/aarch64-rtems7-objcopy -Obinary build/aarch64-rtems7-raspberrypi4b/hello.exe build/kernel8.img
```

Copy `kernel8.img` to your SD card. In the SD card’s `config.txt`, add:

```ini
arm_64bit=1
dtoverlay=disable-bt
```

---

# 📟 Connecting to the Raspberry Pi

**Wiring (via USB-to-UART):**

* Pi Pin 8 (TX) → USB RX
* Pi Pin 10 (RX) → USB TX
* Pi GND → USB GND

Find your USB device:

```bash
ls /dev/cu.*
```

Use `minicom` or any serial terminal at **115200 baud**, **8N1** settings.

You should see something like this:
![RTEMS Hello World on Raspberry Pi](/images/hello-world-rtems-rpi.png)

---

That’s it! You’ve now got RTEMS running on your Raspberry Pi 4B.

Want to know more about my work on this? Check out my [GSoC 2025 RTEMS project](https://summerofcode.withgoogle.com/programs/2025/projects/CUL4f3Nh).

Happy hacking! 🤓

