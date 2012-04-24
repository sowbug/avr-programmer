AVR In-System Programmer
========================

By [Mike Tsao](http://www.sowbug.com/) and [Dennis Gentry](https://github.com/dgentry)

Introduction
============

This is a new spin on the AVR ISP. It's based on the ATmega 'u2 family of AVR devices. Influences while designing, in no particular order:

  * Adafruit's [USBtinyISP](http://www.ladyada.net/make/usbtinyisp/), based on [the original USBtiny project](http://dicks.home.xs4all.nl/avr/usbtiny/). In particular, the buffer came from this project.
  * Fabio Baltieri's [USB Key AVR Programmer](http://fabiobaltieri.com/2011/09/02/usb-key-avr-programmer/). Some early schematic ideas came from here.
  * [Saleae Logic](http://www.saleae.com/logic). Yes, I know that's not an AVR programmer, but I like their design sensibilities.
  * [Frank Zhao](http://frank-zhao.com/) provided feedback on an early version of the circuit.

Design Goals
============

  1. It should be pocketable, or capable of being kept in a mesh backpack compartment, without snagging on anything. This means SMD rather than through-hole, few or no required pin headers, and right-angle headers where headers are required.
  1. No special cables needed. In the common case of a standard 6-pin ISP header on the target board, the programmer needs only a commodity mini-B USB cable. The target audience is DIY people who are designing their own circuits, and in that case it's very unlikely new designs would use a 10-pin ISP header with 4 NC pins.
  1. It should be a real USB device rather than emulating serial over USB, so it needs fewer avrdude options to pass in.
  1. Jumpers should be truly optional. The device should be usable without jumper header pins even soldered onto the board.
  1. Support in current versions of avrdude. With the real-USB-device requirement, this narrows it down to USBasp, USBtiny, or AVRISP mkII emulation.
  1. Inexpensive. To be clear, most AVR programmers are inexpensive, and it's not a goal to be less expensive than them.
  1. Distinctive. The angled 6-pin socket satisfies this goal.
  1. Fast. This should be entirely dependent on the quality of the firmware, because the device itself should be capable of high-speed USB.
  1. Good-looking enough to be presentable without a case.
  1. JTAG/XSVF programming via [The JTAG Whisperer](https://github.com/sowbug/JTAGWhisperer). This means we need to support varying target voltages.
  1. Bring out the serial pins on the ATmega to provide a USB-to-TTL converter with an FTDI-cable-compatible pinout.
  1. Support for all low-voltage programming methods (SPI, TPI, PDI). Note that TPI/PDI turned out to be hard to do with the serial functionality. It should work, but with external resistors and a breadboard.

Notes That Will Someday Become Documentation
============================================

* Fuses are **-U lfuse:w:0xDE:m -U hfuse:w:0xD9:m -U efuse:w:0xF7:m -U lock:w:0xFF:m**. These are default with 16MHz crystal and HWBE=0.

FAQ
===

  * **Err, USBasp? USBtiny? You know those bitbang USB, right? Why consider those protocols if your ATmegaXXu2 has native USB?** It's just a question of avrdude support. Both those protocols are based on open-source hardware, so I'd prefer to base my work on them. Another consideration is that there shouldn't be a problem with borrowing their VID/PID pairs. The bit-banging aspect of the protocols is orthogonal to their suitability.
  * **Your circuit mentions the ATmega8u2. You know the '32u2 is only like $1.00 more, right?** True (actually only 40 cents more in quantity), and the initial prototypes will use the '32u2. But we're going to try to fit the intended functionality into 8KB flash and 512 bytes SRAM. Beyond that, the device is looking more like a general-purpose gadget. If we were building such a thing, the ATmega32u4 (not the 32u2) is a better choice.
  * **About that general-purpose gadget: why aren't you building one? The [Bus Pirate](http://dangerousprototypes.com/docs/Bus_Pirate) is cool.** Yes, the Bus Pirate is cool, and indeed it would be nice to have one built on an open toolchain. But this isn't that device. The intent is to have a specialized tool that's always available, never needing a misplaced cable, never needing a firmware update, during those times when you've bricked an AVR or CPLD and just need it fixed.
