+++
title = "GSoC Midterm evaluation"
date = "2025-07-15T22:29:25+05:30"
author = "Shaunak Datar"
authorTwitter = "datar_shaunak" #do not include @
cover = ""
coverCaption = ""
tags = ["RTEMS", "GSoC", "Raspberry Pi", "Bare-metal", "Device-Drivers"]
keywords = ["GSoC 2025", "RTEMS BSP", "Raspberry Pi 4B", "RTEMS device driver", "I2C PWM DMA Mailbox RTEMS", "Shaunak Datar GSoC"]
description = ""
showFullContent = false
readingTime = true
hideComments = false
color = "" #color from the theme settings
+++

Hello readers!

The GSoC 2025 midterm evaluations are here, and I’d like to take this opportunity to summarize the work I’ve done so far. I’m contributing to the RTEMS Project, focusing on adding and improving peripheral support for the Raspberry Pi 4B BSP. My proposal is centered around four core components: **I2C**, **PWM**, **DMA**, and **Mailbox**. Out of these, I’ve:

- **Merged I2C support**
- **Merged PWM support**
- **Got DMA working for memory-to-memory transfers**

Let’s take a closer look at each component:

---

## I2C

The I2C peripheral on the Raspberry Pi is accessed via the `i2c_bus` layer in RTEMS's `cpukit`. I implemented functions like:

- `rpi_i2c_transfer`
- `rpi_i2c_set_clock`
- `rpi_i2c_destroy`

...along with several helper functions to configure the BSC (Broadcom Serial Controller) registers.

I tested the driver using an MPU6050 sensor and verified correct readings. After testing, I raised a [merge request](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/363).

Most of my time after the testing phase was spent addressing review comments. This helped me understand how to write code that is not only functional, but also **readable**, **maintainable**, and **aligned with RTEMS's coding guidelines**.

---

## PWM

The second part of my project involved adding support for the PWM peripheral on the Raspberry Pi 4B. PWM was relatively straightforward, and I spent around two weeks getting it up and running.

I implemented functions to initialize the PWM master and individual channels, as well as to update the duty cycle of an already-running channel. Since PWM is tightly coupled with the Clock Manager, I also implemented static helper functions to configure the clock source and dividers.

For testing, I wrote a user application that exercised both PWM channels. PWM0 is mapped to GPIOs, so I verified its output using a logic analyzer. PWM1, however, is connected to the audio jack on the Pi. To test this, I configured it to output a 440Hz tone and connected headphones to hear the sound.

After testing, I submitted a [merge request](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/509).

---

## DMA

DMA is the most involving and interesting part of the project so far. Unlike I2C or PWM, it’s not just about configuring registers—it also involves creating a control block, aligning it to a 256-bit address, and setting up memory for source and destination buffers correctly.

Initially, I tested DMA with a simple single-integer transfer, which worked fine on QEMU but failed on actual hardware. After discussions with my mentors, I realized the issue was due to cache coherence—the CPU cache was not in sync with memory when the DMA engine performed the transfer.

To fix this, I modified the memory allocation to ensure proper cache-line alignment, added manual cache flushes and invalidations using RTEMS APIs, and rewrote the test case to use a larger buffer instead of a single integer. With these changes, the memory-to-memory DMA transfer worked reliably on real hardware.

After some restructuring and cleanup, I plan to raise a merge request for DMA support soon.

---

This has been my progress till the midterm. It's been super fun working on this project—I’m learning a lot of new things and growing as an embedded engineer.

That’s it for now.

**Until next time — keep building, keep tinkering, and keep contributing.**  

Cheers,  
Shaunak
