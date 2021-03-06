# plan44 feed for OpenWrt

This is my source feed for building various OpenWrt packages I wrote for projects on Onion Omega1 and Omega2.

If you have a OpenWrt (or LEDE) build environment set up, you can use the plan44 feed by adding the line

    src-git plan44 https://github.com/plan44/plan44-feed.git;master

to your `feeds.conf.default` (or, more correctly, to a copy of `feeds.conf.default` named `feeds.conf`).

## Useful right now

The following packages are of general usefulness right now:

- **p44-ledchain**: this is a kernel module written by me for the MT7688/Omega2 which makes use of the hardware PWM in the chip to drive individually addressable WS281x-type LED chains. In contrast to the *ws2812_draiveris* driver (see below) that does the same for AR9331/Omega1, *p44-ledchain* does **not** block IRQs at all. At this time, *p44-ledchain* is extensively tested with a chain of 200 WS2813, and one of 24 P9823 LEDs. The driver also has modes for WS2811, WS2812 and the RGBW SK6812 LEDs.

- **serialfwd**: a small utility I use in almost every project which allows to access a serial interface via TCP from a remote host, and also to send and receive a few hex bytes directly.

- **ws2812-draivris**: this is a WS281x LED driver that works for AR9331/Omega1, but blocks interrupts for several milliseconds per update, which is *very* bad behaviour for a kernel module. The author of *ws2812-draivris* (not me) is not to blame for this, because the AR9331 chip does not provide any hardware that could generate WS281x timing, so bitbanging and blocking IRQs is the *only* way to do WS281x on a AR9331. Still, it's a bit of a hack that could backfire when other parts of the system rely on fast IRQ response.

- **pagekitec**: a package containing the C implementation of pagekite, which is a reverse proxy that allows reaching embedded targets from the internet (e.g. for service, maintainance, monitoring). Have a look at pagekite.net for information (pagekite is available as a hosted service but also provides the server side as a oppen source python script you can use on your own server)

- **rcswitch**: an extract of the rcswitch kernel module for use in OpenWrt from a bigger project. rcswitch provides a sysfs interface for a connected 434MHz RF module, controlling simple RF power switches.

## complete projects

The **\*-config** packages are "umbrella" packages to be used with the [p44b script](https://github.com/plan44/p44build) for various projects of mine. The older projects are still based on LEDE 17.01.4, the newer ones on OpenWrt 18.06.1.

- **p44ttngw**: a port of a The Things Network (TTN) LoRA gateway to the Omega2, for use with the RAK831 radio.
- **pixelboard**: a lounge table with a Omega2 based tetris and game of life built into the table plate itself.
- **p44sbb**: a clock based on Swiss railway splitflap displays
- **p44wiper**: a DC motor controller for smooth "wiping" movements of a DC motor with a single reed contact to avoid drift.
- **p44bandit**: a serial interface controller for a old BANDIT CN milling machine.
- **hermel** and **leth**: firmware for a Omega2S based controller using the PWM outputs for driving WS2813 LED chains for some effects and a scrolling text ticker, and a 2-channel A/D converter for sensor input, used for exhibition objects.
 
## Work in progress!

There is and will always be some work-in-progress in this feed, so don't expect everything turnkey ready.

Note that *i2c-tools* and *libpng* are also available in the standard feeds, but in slightly different variations - I needed some modifications for my own projects so I duplicated the packages. Fortunately, OpenWrt source feed management is prepared for having multiple versions of the same package - just use the -p option to choose the feed to install a particular package from:

    ./scripts/feeds update plan44
    ./scripts/feeds install -p plan44 libpng


