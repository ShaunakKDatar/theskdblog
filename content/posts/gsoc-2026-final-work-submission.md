+++
title = "GSoC 2026 Final Work Submission"
date = "2026-08-22T00:00:00+05:30"
author = "Shaunak Datar"
authorTwitter = "datar_shaunak" #do not include @
cover = ""
coverCaption = ""
tags = ["RTEMS", "GSoC", "Raspberry Pi", "Bare-metal", "Device-Drivers"]
keywords = ["GSoC 2026", "RTEMS BSP", "Raspberry Pi 4B", "RTEMS device driver", "Shaunak Datar GSoC"]
description = ""
showFullContent = false
readingTime = true
hideComments = false
color = "" #color from the theme settings
+++

Project Proposal: https://docs.google.com/document/d/1yuoWpMpwLSp4h_1vC6Ji5qHPJlRXdbX1orn8oMU-BDE/edit?usp=sharing

Gitlab Activity: https://gitlab.rtems.org/users/skdatar/activity

Project Issue: https://gitlab.rtems.org/rtems/programs/gsoc/-/work_items/89

# Code contributions:
| Merge Request | Description | Merged? |
|---------------|--------|--------|
| [Framebuffer Support](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/1340) | Add the framebuffer support | ✅ |
| [SD Card support RTEMS](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/1429) | Add the mmc node to the dts | ✅ |
| [SDHCI Support in libbsd](https://gitlab.rtems.org/rtems/pkg/rtems-libbsd/-/merge_requests/158) | Two commits to import the SDHCI files to libbsd and port them for RTEMS | ✅ |

# Aim of the project

The project aimed to add Framebuffer, SD card and PCIe support to the Raspberry Pi Board Support Package

## Framebuffer

The work related to the framebuffer heavily relied on the support for [mailbox](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/915), which I added earlier in the year. The Raspberry Pi has an interesting [boot sequence](https://rxos.readthedocs.io/en/develop/how_it_works/boot.html#raspberry-pi-boot). The VideoCore GPU handles most of the board's low-level functionality, such as power and clock management. The mailbox interface provides the mechanism for the CPU to talk to the GPU.
The CPU can draft a mail and send it over to the GPU using a mailbox. [The property interface](https://github.com/raspberrypi/firmware/wiki/Mailbox-property-interface) is a way for the CPU to GET/SET information from the VideoCore.

The framebuffer support added uses these property tags: [framebuffer-specific tags](https://github.com/raspberrypi/firmware/wiki/Mailbox-property-interface#frame-buffer). More on that in a more detailed blog later.
The framebuffer was tested using distinct red, green and blue colour fills:


Red: ![Red](/images/fb_red.png)
Green: ![Green](/images/fb_green.png)
Blue: ![Blue](/images/fb_blue.png)

Merge Request: https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/1340

## SDHCI Support
The BCM2711 has three SD controllers — SDHOST, SDHCI and EMMC2 — all of which reach the SD card slot through a hardware mux, except the SDHOST. On the Pi 4, the SD card slot is driven directly by `bcm2835_sdhci.c`.
The work for SDHCI support was added in three separate commits across the RTEMS and libbsd repositories.

We initially added the mmc node to the `bcm2711-rpi-4-b.dts`. Later, the libbsd commits imported the SDHCI-specific files and then ported them to RTEMS to use the mailbox interface defined in RTEMS. The porting also involved using PIO mode instead of DMA, because peripheral-based DMA is yet to be added in RTEMS.

SDHCI support was tested using the libbsd media01 test:
![Test](/images/libbsd_media01_test.png)

Merge Request:

DTS MR: https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/1429

libbsd MR: https://gitlab.rtems.org/rtems/pkg/rtems-libbsd/-/merge_requests/158

# Future Work
- The PCIe support was proposed as a part of this project but could not be completed due to time constraint. After GSoC I plan to work on adding the PCIe support
- Adding the peripheral facing DMA is also an open future work item.
- Adding support for USB is also a next step for the BSP

# Conclusion
The time I dedicated to working on the Raspberry Pi BSP, helped me gain a deeper understanding 
of the architecture and the hardware. It sharpened my coding skills, helped me understand open-source.
I would like to thank my mentor: Christian and Kinsey for putting in the time and effort, helping me understand good coding practices, understand the intricacies of the hardware and reviewing my MRs.
A huge thank you to Google for connecting students and contributors like me to organisations like RTEMS.