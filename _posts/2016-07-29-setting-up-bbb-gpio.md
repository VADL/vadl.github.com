---
title: Setting Up the BeagleBone Black's GPIO Pins
date: '2016-07-29 07:27:00'
tags: bbb, gpio,
categories: beagleboneblack
layout: post
---


This post will detail how to set up the BBB's GPIO pins. General
Purpose Input/Output (GPIO) pins are special in that they can be
configured at runtime to perform in a variety of ways, ranging from
simple i/o to serial interfaces to specialized encoder readings. While
the BBB supports up the 69 gpio pins, in reality the majority of the
pins are being used by onboard system processes such as the board's
HDMI and LCD abilities. This post will detail the steps necessary to
take advantage of these otherwise inaccessible pins, as well as
configure a gpio pin to suit the user's needs.

## Linux Kernel Version

This guide assumes the user's BBB is running _Linux Kernel 4.1.15_. If
an earlier version is used, such as _3.8.X_, then abundant information
on configuring the BBB can be found through google and the procedures
listed in this guide will probably not work.

## BBB Headers and Pinout

The BBB has two columns 23 pins each on either side of the board, for
a total of 96 pins available to the user. The right header is
designated as P8 while the left is designated P9. Pay careful
attention to the orientation of the BBB in reference to the pinout
numbers, else you risk mis-wiring the board and potentially causing
irreversible damage. Physical pins are numbering in the following
manner `PX_Y` where `X` is the header where the pin is located (8 or
9) and `Y` is the location of the pin with that header. Pins _P8\_1_
and _P9\_1_ are located at the top right of each header. Notice that
the left column of each header are all the odd pins while the right
column of each header are all the even pins.

![BBB Headers](/images/bbb/bbb_headers.png){: .center-image }

As seen in the graphic, the first and last rows of pins are dedicated
to nets such as _DGND_, _VDD\_3V3_ (3.3V Output), _VDD\_5v_ (5V
Output), and system power/reset. The rest of the pins are configurable
in software, as will be detailed later in this post. The labels on the
graphic show the _default mode_ of the pins. Not all pins are able to
be used as GPIO's immediately. Systems such as the LCD and HDMI
drivers take priority to these pins. A more detailed view of the BBB's
header's and pin usage can be seen in the graphics below

![P8 Header](/images/bbb/P8Header.png){: .center-image }
![P9 Header](/images/bbb/P9Header.png){: .center-image }

### GPIO Numbering Scheme

The gpio pins of the bbb are grouped into 3 groups of 32: _GPIO0_,
_GPIO1_, and _GPIO2_. An individual pin can be refered to using the
convention `GPIOX_Y` where `X` is its gpio register and `Y` is its
number within that register. _However, all references to a particular
pin made in software instead uses its absolute pin number!_ A gpio's
_absolute_ pin number is calculated in the following manner: `Z =
32*X + Y` where `X` is again the gpio register and `Y` is the position
within that register.

i.e. GPIO2\_24 is `32*2+24`, making it GPIO\_88. _If this pin were to be
referenced anywhere in software, the user would use the number 88, not
24_!

### Overloaded Pins

Additional scrutiny of the above header pin layouts shows all the
possible uses of each individual pin. Any pin highlighted in red is a
pin that is inaccessible for use as a gpio pin by default, see the notes column for it's initial allocation. As an example, P8\_28, aka GPIO2\_24, aka GPIO\_88, is by default allocated to the `nxp_hdmi_bonelt_pins` group. While it is overloaded in this capacity, no other use can be made of this pin. Multiple modes are available for each pin, and setting these modes wil be discussed in a later section.

### Recap of Numbering Schemes

As a recap, _each gpio pin on the BBB has three different numbering schemes associated with it!

1. The physical pin location, in the form of `PX_Y` (P8\_28)
2. The gpio name, in the form of `GPIOX_Y` (GPIO2\_24)
3. The gpio number, in the form of `32*X + Y` (88)

Only the last scheme, the gpio number, is used in software!

## Configuring Accessible GPIO Pins

```bash
sudo su
```

```bash
cd /sys/class/gpio
```

```bash
echo 27 > export
```

```bash
cd gpio27
```

```bash
echo out > direction
```

```bash
cat value
```

```bash
echo 1 value
```

### Access using C++ library

The C++ library can be found [here.](github.com/rosmod/lib-bbbgpio)

```c++
#include "gpio.h"
~~~
~~~
unsigned int pin = 27;
gpio_export(pin);
gpio_set_dir(pin,OUT);
gpio_set_value(pin,1);
```



## Accessing Inaccessible Pins

Before going into the details of configuring inaccessible pins, it is
necessary to briefly go over the inner workings of the BBB and touch
on concepts such as pin modes, the device tree and its overlays, and
how to interact with it.

### Pin Modes


![GPIO_Bits](/images/bbb/GPIO_Bits.png){: .center-image }

### Device Tree 

[Here](https://learn.adafruit.com/introduction-to-the-beaglebone-black-device-tree/overview
(Outdated)) is a good overview of what exactly the device tree
is. _BEWARE, THIS LINK IS OUTDATED AND IS NO LONGER COMPLETELY ACCURATE!_ 

The Linux Device tree controls the configuration of pins: mux mode,
direction, pullup/pulldown, etc. It is a generic, standardized way of
dealing with constantly updating hardware configurations. In short, it
describes the hardware in a system. It is the linux version of ARM
board files.

The device tree is compiled into a `.dtb` file that is then loaded
upon the kernel's startup. All specified pins and peripherals are
loaded and configured for use in the system.



### Cape Manager and Device Tree Overlays

As mentioned in the previous section, the device tree is loaded when
the kernel starts. It would be a very long and tedious process if, in
order to make any changes to the device tree, one were required to
modify the device tree, recompile it, and restart the device. Luckily,
the BBB's cape manager comes to the rescue. The cape manager allows
the modification of parts of the device tree _at runtime_ by loading
_overlays_ that enable peripherals or mux pins to a specific function.

To see what device overlays are currently loaded on the device:

```bash
cat /sys/devices/platform/bone_capemgr/slots
```

As an example, one might see the following:

```bash
root@node31:~# cat /sys/devices/platform/bone_capemgr/slots
0: PF----  -1 
1: PF----  -1 
2: PF----  -1 
3: PF----  -1 
4: P-O-L-   0 Override Board Name,00A0,Override Manuf,BB-ADC
5: P-O-L-   1 Override Board Name,00A0,Override Manuf,gpio_88
```

This shows that two overlays, BB-ADC and gpio_88, have been loaded by the cape manager.

In order to check if an overlay has successfully changed the pins modes of the target pins, utilize the following command:

```bash
cat /sys/kernel/debug/pinctrl/44e10800.pinmux/pins
```

`/sys/kernel/debug/pinctrl/44e10800.pinmux/pins` will provide the pin
number, the pin's physical memory address, and the current mode to
which the pin has been muxed. 

```bash
cat /sys/kernel/debug/pinctrl/44e10800.pinmux/pingroups | less
````

`/sys/kernel/debug/pinctrl/44e10800.pinmux/pingroups` provides a list of all "claimed" pins, such as pins that are configured and reserved via overlays.

### DTC And Compiling Custom Overlays

In order to make custom overlays, it in necessary to install the device
tree overlay compiler made and maintained by Robert Nelson. It can be
found at
[https://github.com/beagleboard/bb.org-overlays](https://github.com/beagleboard/bb.org-overlays). For
ease of use, we have forked the current version at
[https://github.com/VADL/bb.org-overlays](https://github.com/VADL/bb.org-overlays),
so that as long as you are running the _4.1.15_ kernel, compatibility
is not an issue.

To install the compiler, perform the following steps.

1. clone the dtc repo
```bash
git clone https://github.com/vadl/bb.org-overlays
cd ./bb.org-overlays
```

2. Run the dtc-overlay script
```bash
./dtc-overlay.sh
```

After installation, the dtc compiler can be called on a .dts file to
produce the corresponding .dtbo overlay file. For an example overlay
called _sample\_overlay.dts_:

```bash
dtc -O dtb -o /lib/firmware/sample_overlay-00A0.dtbo -b 0 -@ sample_overlay.dts
```

Be sure to put the new .dtbo file into `/lib/firmware` so that it is visible to the BBB os.

One minor yet crucial tidbit of knowledge to remember is that the overlay
name cannot exceed 14 characters! If this limit is exceeded then it
will not be able to be enabled by the BBB os.


### Disabling Default Overlays

### Enabling and Disabling Custom Overlays

## Related Concepts

### PRU

Reading and writing the gpio pins through the file system as detailed
above is a relatively slow process, capable of read/write frequencies
in the low hundreds of Hz. An alternative is to utilize the BBB's built in
Programmable Realtime Units (PRU's). PRU's are independent units
completely separate from the BBB OS, and can allow I/O at up to 20
MSPS. While this guide does not detail how to use a PRU, don't forget
it exists!

[A GitHub repo supporting PRU use can be found here](https://github.com/beagleboard/am335x_pru_package)


### Analog I/O

### Serial and UART Communication

##Links

A good writeup about BBB Overlays and the cape manager can be found [here](http://www.thing-printer.com/cape-manager-is-back-baby/)
