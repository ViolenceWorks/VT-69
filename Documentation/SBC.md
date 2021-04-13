# Setting Up a Single Board Computer

The VT-69 includes an internal 40-pin header for attaching a single board computer (SBC), expansion board, or other electronics to expand the functionality of the VT-69. The inspiration for this internal header comes from the DEC VT-180 terminal, basically a VT-100 terminal with an extra board installed which included a Z80 processor, 64k of RAM, a floppy disk controller and an extra serial port controller. This addition made the VT-180 a dedicated personal computer using the CP/M operating system.

## The Internal Header

The internal 40-pin header follows a very common layout, with 3.3V on pin one, 5V power input on pins 2 and 4, etc... The Rx and Tx pins (from microcontroller to header) are pins 8 and 10, respectively, and there are provisions for an optional RTS and CTS connections through pins 31 and 17, resepectively.

![Diagram of pin header](https://github.com/ViolenceWorks/VT-69/blob/main/Documentation/ArtAssets/40PinHeader.png)

Pins 2 and 4 are the only power supplies for the header, providing 5V. Pin 1 is treated as an output on the header; The VT-69 may be configured to default to the internal header if the microcontroller detects a 3.3 logic level high on this pin during reset.

**Important** If you develop your own extention to the VT-69, your board must _supply_ 3.3V to the VT-69 over pin 1 for automatic detection of the board to occur during boot. 

## Configuring a Linux-based Single Board Computer

This guide assumes the SBC is running at least Debian 'Jessie', kernel version 4 or above. This guide assumes `systemd`; Earlier versions (Wheezy) will not work with this guide. The non-systemd method was six years old when this document was created and will not be supported. If you are running an older version Debian, Google `/boot/cmdline.txt` and fuck around with that. 

1. Enable login shell over serial

	From the console, type `sudo raspi-config`. Enable login shell over serial.

2. Edit the serial-getty service

	change the line `ExecStart=-/sbin/agetty -o '-p -- \\u' --keep-baud 115200,38400,9600 %I $TERM` to the following: `ExecStart=-/sbin/agetty 9600 %I vt100`.

3. Restart systemctl

	`systemctr daemon-reload`

4. Reboot the system.

	On reboot, you should see a stream coming from the VT-69's internal header. After the Pi boots, you will be able to login. If you do not see a login propmpt, change the terminal settings to 9600 baud.
