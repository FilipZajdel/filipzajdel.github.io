---
layout: post
title: What is Linux about?
---

Dig into the kernel's architecture.

When we hear about Linux our thoughts are probably somewhere between convoluted command line interface and that cute penguin mascotte. In general, Linux is seen as something strange and awkward. In reality it is even worse and
you are going to know why in a second.

Firstly, imagine who is responsible for providing a cute kitties from the internet cable to your browser.
You are right- some software. The software responsible for doing all the magic with bits, bytes and streams tangled in the hardware is the operating system and- in more depth- device drivers.

Each part of the computer, whatever it is: a GPU, hard drive or floppy disk drive needs a specific piece of software which knows all its behaviors, needs and issues. In technical terms- a device driver is a piece of software which runs in the operating system and controls the specified hardware in means of sending the data back and forth, handling interrupts and providing a simple, yet reliable interface for the user.

**Note:** The term *user* in the means of this tutorial is not meant to indicate a person who uses software, but rather a computer engineer who uses the device drivers in his programs.
