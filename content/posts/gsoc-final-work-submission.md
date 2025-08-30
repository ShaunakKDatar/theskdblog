+++
title = "Gsoc Final Work Submission"
date = "2025-08-29T19:06:32+05:30"
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

Project Proposal: https://docs.google.com/document/d/1NreikYpimpCXKtAVqa8RMzbgOyQgtGw7x1CQlPVAtJY/edit?usp=sharing

Gitlab Activity: https://gitlab.rtems.org/users/skdatar/activity

Project Issue: https://gitlab.rtems.org/rtems/programs/gsoc/-/issues/81

# Code contribution list:
| Merge Request | Description |
|---------------|--------|
| [I2C (Polling)](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/363) | Implemented polling-based I2C driver. |
| [I2C (Interrupt)](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/682) | Added TX interrupt support for I2C. |
| [PWM Support](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/509) | Implemented PWM driver, tested with logic analyzer & audio jack. |
| [DMA Support](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/662) | Added standard DMA, DMA Lite & DMA4 support (memory-to-memory). |
| [Docs (PWM)](https://gitlab.rtems.org/rtems/docs/rtems-docs/-/merge_requests/178) | Documentation for PWM driver. |
| [Docs (DMA)](https://gitlab.rtems.org/rtems/docs/rtems-docs/-/merge_requests/186) | Documentation for DMA driver. |
| [Header Guards](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/665) | Added C++ header guards to BSP header files. |
| [Formatting Fix (raspberrypi.h)](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/526) | Fixed formatting issues in `include/bsp/raspberrypi.h`. |
| [Docs (I2C)](https://gitlab.rtems.org/rtems/docs/rtems-docs/-/merge_requests/190) | Documentation for I2C driver. |

# Aim of the project

The project aimed to have the support for I2C, PWM, DMA and Mailbox added to the Raspberry Pi 4b BSP.

## I2C Support

The I2C support was upstreamed in two separate MRs. One which implemented the Polling version.
Another which added the TX Interrupt support to the driver.

I2C was tested on the MPU6050 as well as the logic analyser

![MPU6050 using I2C](/images/MPU6050_I2C.png)

![I2C on Logic analyser](/images/I2C_LA.jpeg)

Polling Merge Request: https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/363

Interrupt Merge Request: https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/682

## PWM Support

The PWM support was added and was tested using a logic analyser
![PWM Testing Output](/images/PWM_Output.jpeg)

PWM1_x channels are routed to the audio jack. PWM1 was then tested using the Audio Output on the RaspberryPi

PWM Merge Request: https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/509

## DMA Support

The DMA Support was a very interesting part. During the coding period I could
complete the memory-to-memory transfer.
It involved a lot of cache management and handling control block structures.

Apart for the standard DMA there are two other types of DMA: DMA Lite and DMA4

All of these were tested for memory-to-memory transfer.

The standard DMA test:
![DMA Testing](/images/DMA-std.png)

DMA Lite test:
![DMA Lite Testing](/images/DMA-Lite.png)

DMA4 test:
![DMA4 Testing](/images/DMA4.png)

DMA Merge Request: https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/662

## Mailbox Support

Preliminary work on the Mailbox was started, though not completed in the coding period.

## Other Merged MRs

- [Documentation for DMA](https://gitlab.rtems.org/rtems/docs/rtems-docs/-/merge_requests/186)
- [Documentation for PWM](https://gitlab.rtems.org/rtems/docs/rtems-docs/-/merge_requests/178)
- [Adding C++ header guards for the BSPs header files](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/665)
- [Fix the formatting for the include/bsp/raspberrypi.h file](https://gitlab.rtems.org/rtems/rtos/rtems/-/merge_requests/526)
- [Documentation for I2C](https://gitlab.rtems.org/rtems/docs/rtems-docs/-/merge_requests/190)

# Future Work

There is considerable work to be done on the Raspberry Pi BSP. The support for Mailbox is incomplete,
the sdhci support from GSoC 2024 still remains to be completed. The BSP could also use support for Ethernet and USB. 

I plan to continue working on the Raspberry Pi BSP in particular with RTEMS in general.

# Conclusion

The time I dedicated to working on the Raspberry Pi BSP, helped me gain a deeper understanding 
of the architecture and the hardware. It sharpened my coding skills, helped me understand open-source.
I would like to thank my mentor: Kinsey and Christian for putting in the time and effort, helping me understand good coding practices, understand the intricacies of the hardware and reviewing my MRs.
A huge thank you to Google for connecting students and contributors like me to organisations like RTEMS.