AVR In-System Programmer
========================

By Mike Tsao [www.sowbug.com](http://www.sowbug.com/)

Introduction
============

This is a new spin on the AVR ISP. It's based on the ATmegaU2 family of AVR devices. My influences while designing, in no particular order:

  * Adafruit's [USBtinyISP](http://www.ladyada.net/make/usbtinyisp/), based on [the original USBtiny project](http://dicks.home.xs4all.nl/avr/usbtiny/).
  * Fabio Baltieri's [USB Key AVR Programmer](http://fabiobaltieri.com/2011/09/02/usb-key-avr-programmer/).
  * [Saleae Logic](http://www.saleae.com/logic). Yes, I know that's not an AVR programmer, but I like their design sensibilities.
  
Design Goals
============

  1. It should be pocketable, or capable of being kept in a mesh backpack compartment, without snagging on anything. This means SMD over through-hole, few or no pin headers, and right-angle headers where any headers are required.
  1. No special cables needed. In the common case of a standard 6-pin ISP header on the target board, the programmer needs only a commodity mini-B USB cable.
  1. It should be a real USB device rather than emulating serial over USB, so it needs fewer avrdude options to pass in.
  1. Jumpers should be truly optional. The device should be usable without jumper header pins even soldered onto the board.
  1. Support in current versions of avrdude. With the real-USB-device requirement, this narrows it down to USBasp or USBtiny emulation.
  1. Inexpensive (though to be clear, most AVR programmers are inexpensive, and it's not a goal to be less expensive than them).
  1. Distinctive. The angled 6-pin socket satisfies this goal.
  1. Fast. This should be entirely dependent on the quality of the firmware, because the device itself should be capable of high-speed USB.
  1. Good-looking enough to be presentable without a case.
  1. JTAG/XSVF programming via [The JTAG Whisperer](https://github.com/sowbug/JTAGWhisperer). This means we need to support varying target voltages.

FAQ
===

  * **Err, USBasp? USBtiny? You know those bitbang USB, right? Why consider those protocols if your ATmegaXXu2 has native USB?** It's just a question of avrdude support. Both those protocols are based on open-source hardware, so I'd prefer to base my work on them. Another consideration is that there shouldn't be a problem with borrowing their VID/PID pairs. The bit-banging aspect of the protocols is orthogonal to their suitability.
  * **Your circuit mentions the ATmega8u2. You know the '32u2 is only like $1.00 more, right?** True (actually about 40 cents in quantity). But it ought to be possible to fit the intended functionality into 8KB flash and 512 bytes SRAM. Beyond that, the device is looking more like a general-purpose gadget, for which the ATmega32u4 (not the 32u2) is a better choice.
