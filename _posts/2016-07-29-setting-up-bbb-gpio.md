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

# Linux Kernel Version

This guide assumes the user's BBB is running _Linux Kernel 4.1.15_. If
an earlier version is used, such as _3.8.X_, then abundant information
on configuring the BBB can be found through google and the procedures
listed in this guide will probably not work.

# BBB Headers and Pinout

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

## GPIO Numbering Scheme

The gpio pins of the bbb are grouped into 3 groups of 32: _GPIO0_,
_GPIO1_, and _GPIO2_. An individual pin can be refered to using the
convention `GPIOX_Y` where `X` is its gpio register and `Y` is its
number within that register. _However, all references to a particular
pin made in software instead uses its *absolute* pin number!_ A gpio's
_*absolute*_ pin number is calculated in the following manner: `Z =
32*X + Y` where `X` is again the gpio register and `Y` is the position
within that register.

i.e. GPIO2_24 is `32*2+24`, making it GPIO_88. _If this pin were to be
referenced anywhere in software, the user would use the number 88, not
24_

## Overloaded Pins

Additional scrutiny of the above header pin layouts shows all the
possible uses of each individual pin. Any pin highlighted in red is a
pin that is inaccessible for use as a gpio pin by default, see the notes column for it's initial allocation. As an example, P8\_28, aka GPIO2\_24, aka GPIO\_88, is by default allocated to the `nxp_hdmi_bonelt_pins` group. While it is overloaded in this capacity, no other use can be made of this pin. Multiple modes are available for each pin, and setting these modes wil be discussed in a later section.

## Recap of Numbering Schemes

As a recap, _each gpio pin on the BBB has three different numbering schemes associated with it!*

1. The physical pin location, in the form of `PX_Y`
2. The gpio name, in the form of `GPIOX_Y`
3. The gpio number, in the form of `32*X + Y`

Only the last scheme, the gpio number, is used in software!

# Configuring Accessible GPIO Pins

# Accessing Inaccessible Pins

## Pin Modes

## Device Tree Overlay

## BBB Capes and Cape Manager

## DTC And Compiling Custom Capes

## Disabling Default Capes

## Enabling Custom Capes

# Related Concepts

## PRU

## Analog I/O

## Serial and UART Communication
