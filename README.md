AVR In-System Programmer
========================

By Mike Tsao [www.sowbug.com](http://www.sowbug.com/)

Introduction
============

This is a new spin on the AVR ISP. My influences while designing, in no particular order:

  * Adafruit's [USBtinyISP](http://www.ladyada.net/make/usbtinyisp/), based on [the original USBtiny project](http://dicks.home.xs4all.nl/avr/usbtiny/)
  * Fabio Baltieri's [USB Key AVR Programmer](http://fabiobaltieri.com/2011/09/02/usb-key-avr-programmer/)
  * [Saleae Logic](http://www.saleae.com/logic). Yes, I know that's not an AVR programmer, but I like their design sensibilities.
  
Design Goals
============

  1. It should be pocketable, or capable of being kept in a mesh backpack compartment, without snagging on anything.
  1. No special cables needed. In the common case of a standard 6-pin ISP header on the target board, the programmer needs only a commodity mini-B USB cable.
  1. USB device rather than serial-over-USB; fewer avrdude options to pass in.
  1. Jumpers should be truly optional. The device should be usable without the header pins even soldered onto the board.
  1. Support in avrdude. With the real-USB-device requirement, this narrows it down to USBasp or USBtiny emulation.
  1. Inexpensive (though most AVR programmers are already fairly cheap these days; it's not a goal to further undercut those prices).
  1. Distinctive. The angled 6-pin socket satisfies this goal.
  1. Fast. This should be entirely dependent on the quality of the firmware, because the device itself should be capable of high-speed USB.
  1. Good-looking enough to be presentable without a case.

Non-Goals
=========

  1. Simply copying an existing circuit.
  1. Building a multipurpose device.
  1. Explicitly supporting unusual voltages. (I might revisit this goal; it's potentially a nice feature of the USBtiny, though that device's buffer prevents self-programming, which is necessary for this TQFP chip.)

Note that if we do end up supporting other voltages, it would be easy to add headers for [JTAG/XSVF programming](https://github.com/sowbug/JTAGWhisperer). This would conflict with Non-Goal #2.