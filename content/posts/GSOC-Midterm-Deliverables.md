+++
title = "Bits to BSPs – GSoC Midterm Recap"
date = "2025-06-25T00:48:25+05:30"
author = "Shaunak Datar"
authorTwitter = "datar_shaunak" # do not include @
cover = ""
coverCaption = ""
tags = ["RTEMS", "GSoC", "Raspberry Pi", "Bare-metal", "Device-Drivers"]
keywords = ["GSoC 2025", "RTEMS BSP", "Raspberry Pi 4B", "RTEMS device driver", "I2C PWM DMA Mailbox RTEMS", "Shaunak Datar GSoC"]
description = "A midterm progress report on my GSoC 2025 project with RTEMS, detailing the work done so far and what lies ahead."
showFullContent = false
readingTime = true
hideComments = false
color = "" # color from the theme settings
+++

Hello readers!

We’re about four weeks into GSoC 2025, and it's been an incredible ride so far. I'm thrilled to be working with the RTEMS Project this summer. In this post, I want to walk you through what I’ve been working on, what’s already done, and what’s coming up before the midterm evaluations.

---

## What am I going to talk about?

- What project am I working on for GSoC 2025?
- What have I accomplished so far?
- What do I plan to complete before midterm evaluations?

---

## The Project

I'm contributing to the project:  
**[Adding I2C, PWM, DMA and Mailbox Support to the Raspberry Pi 4B BSP in RTEMS](https://summerofcode.withgoogle.com/programs/2025/projects/CUL4f3Nh)**

As the title suggests, the goal is to implement and upstream support for the following peripherals in the Raspberry Pi 4B Board Support Package (BSP) for RTEMS:

- I2C
- PWM
- Mailbox
- DMA

### Why these peripherals?

There are two perspectives here:

**One**, I2C and PWM are fundamental interfaces. Many BSPs already support them – for example, I2C in [arm/raspberrypi](https://gitlab.rtems.org/rtems/rtos/rtems/-/tree/main/bsps/arm/raspberrypi/i2c) and PWM in [arm/beagle](https://gitlab.rtems.org/rtems/rtos/rtems/-/tree/main/bsps/arm/beagle/pwm).  
**Two**, DMA and Mailbox are essential for enabling SDHCI support, which was a pending item from last year’s RTEMS efforts.

A more personal reason: I2C and PWM are what powered the self-balancing feature of the [Wall-E robot from SRA](https://github.com/SRA-VJTI/Wall-E). I first learned about them back in my first year at college, taught First Year students about this robot in my second year — and now, getting to implement them at RTEMS feels increadibly fulfilling.

---

## Work Done So Far

These weeks have flown by! I've written and debugged a lot of code, fixed formatting issues, and had two merge requests accepted. It’s been a fast-paced journey, and I’m incredibly grateful to my mentors, Christian and Kinsey, for their guidance and quick feedback.

### What’s been merged?

- ✅ **PWM Driver for Raspberry Pi 4B**:  
  [Merged Commit](https://gitlab.rtems.org/rtems/rtos/rtems/-/commit/d4755476bcc95c1a1864fa20c1fc7255f509074a)  
  The driver allows users to configure both PWM controllers (PWM0 and PWM1) and their respective channels. A detailed blog post on usage and design is on the way—stay tuned!

- ✅ **Formatting fixes** for the BSP header:  
  [raspberrypi.h](https://gitlab.rtems.org/rtems/rtos/rtems/-/commit/1c9e91d25bdc7175a41b88c306d7ed03697dc66f#f6733755e37b7dc68c7ae64c1e9ec3e1b8a19511)

### Other Contributions

- Re-tested the I2C driver and handled an edge case involving 0-length transfers.
- Started digging into the DMA controller documentation to prep for the next phase.

---

## Plan Before Midterm Evaluations

After syncing up with my mentors, here’s what we’ve decided as the midterm deliverables:

- [x] I2C driver merged upstream
- [x] PWM driver merged upstream
- [ ] Significant progress on either DMA or Mailbox support

The PWM and I2C work has been throughly tested and upstreamed. I have started work on implementing the DMA controller.

With 2–3 weeks left until evaluations, I'm focused and hopeful. Let’s keep building and contributing to this wonderful ecosystem called **open source**.

---

That’s it for now — but there’s a lot more coming soon!
If you’re into embedded systems, RTEMS, or just love geeking out over low-level stuff, I’d love to chat. Feel free to reach out anytime!

Until next time — keep building, keep tinkering, and keep contributing.  

Cheers,  
Shaunak
