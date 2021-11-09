# Arduino Uno as a Nintendo Switch controller

control your Nintendo Switch using an Arduino Uno

## requirements

- Arduino Uno R3
- FTDI USB to UART serial adapter
- USB B to USB A cable (comes with Arduino)
- 4 jumper wires

## installation

Note: This project still has many problems on Windows. I used Ubuntu 18.04.
Download the ZIP file or clone this project.
Download the LUFA project at the bottom of this page http://www.fourwalledcubicle.com/LUFA.php
Change the LUFA path in the included makefile, as well as setting the MCU value to atmega16u2

## assembly

the assembly is fairly straightforward, here is a rough diagram of the parts
and how they will be hooked up when operating

```
                        [your computer]
                            |
          +===========+     |
          | (FTDI)    |-----+ 
          | gnd rx tx |
          +==-===-==-=+
          |  |   |  |
          |  |   |  |
          |  |   |  |
          |  |   |  |  wires (note: tx matches with rx (crossed))
          |  |   |  |
    +-==-====-===-==-==+
    |    5V gnd  rx tx |-------------+  (usb cable)
    |   (arduino uno)  |             |
    +==================+        [nintendo switch]

```

my setup

## building

```bash
make MCU=atmega16u2
```

use the appropriate `MCU` for your board, the Uno uses `atmega16u2`, the pro micro uses `atmega32u4`

Note: If you get an error, you likely don't have some of the tools installed. 
Run `sudo apt-get install gcc-avr binutils-avr avr-libc` to fix this.

## flashing

WINDOWS
-----------------------------------------------------------------------------
- Download Flip (https://www.microchip.com/en-us/development-tool/flip).
- Connect the Arduino to your computer with the USB cable
- You'll see a 3x2 rectangle of pins at the top right, if the Uno's USB port is on the top. Connect the top right pin to GND briefly.
- Select the USB device and flash the HEX file onto the Uno. It's now considered to be a Pokken controller.

UBUNTU
------------------------------------------------------------------------------
you have to be quick with this!

- connect the Arduino to your computer
- short `rst` to `gnd` twice in quick succession

```bash
sudo avrdude -v -patmega32u4 -cavr109 -P/dev/ttyACM0 -Uflash:w:output.hex
```

use the appropriate `MCU` and serial port for your board, the pro micro uses
`atmega32u4` and `/dev/ttyACM0`

## usage

to use the controller, simply connect everything like the diagram shows, then send an input via the serial port

commands are single-byte ascii characters sent over 9600 baud serial.

this is the current list of commands:

```
0: empty state (no buttons pressed)
A: A is pressed
B: B is pressed
X: X is pressed
Y: Y is pressed
H: Home is pressed
C: Capture is pressed
+: + is pressed
-: - is pressed
L: left trigger is pressed
R: right trigger is pressed
l: ZL is pressed
r: ZR is pressed

left stick directions:

      w
   q     e
      ║
a  ═══╬═══  d
      ║
   z     c
      s
      
      
right stick directions:

      1
   5     6
      ║
2  ═══╬═══  4
      ║
   7     8
      3
      
DPAD directions:

      t
         
      ║
g  ═══╬═══  j
      ║
         
      n
      
```

## thanks
Thanks to asottile for putting most of this together, and to javmarina for the help with the setup.

By extension,
Thanks to Shiny Quagsire for his [Splatoon post printer](https://github.com/shinyquagsire23/Switch-Fightstick) and of course to progmem for his [original discovery](https://github.com/progmem/Switch-Fightstick).
Also thanks to bertrandom for his [snowball thrower](https://github.com/bertrandom/snowball-thrower) and all the modifications.
