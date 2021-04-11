# Setting Up a Single Board Computer

The VT-69 includes an internal 40-pin header for attaching a single board computer (SBC), expansion board, or other electronics to expand the functionality of the VT-69. The inspiration for this internal header comes from the Digital VT-180 terminal, basically a VT-100 terminal with an extra board installed which included a Z80 processor, 64k of RAM, a floppy disk controller and an extra serial port controller. This addition made the VT-180 a dedicated personal computer using the CP/M operating system.

## The Internal Header

The internal 40-pin header follows the 'Raspberry Pi' format, with 3.3V on pin one, 5V power input on pins 2 and 4, etc... The Rx and Tx pins (from microcontroller to header) are pins 8 and 10, respectively, and there are provisions for an optional RTS and CTS connections through pins 3 and 5, resepectively.

![Diagram of pin header](https://github.com/ViolenceWorks/VT-69/blob/main/Documentation/ArtAssets/40PinHeader.png)

Pins 2 and 4 are the only power supplies for the header, providing 5V. Pin 1 is treated as an output on the header; The VT-69 may be configured to default to the internal header if the microcontroller detects a 3.3 logic level high on this pin during reset.

**Important** If you develop your own extention to the 

## Configuring a Linux-based Single Board Computer

This guide assumes the SBC is a Raspberry Pi zero. 