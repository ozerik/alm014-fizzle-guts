# M4TM FZilter

![picture of my FZilter module](FZilter.png)

## Introduction

Here's my take on the excellent Fizzle Guts module by ALM/BusyCircuits. It rules.

I've redrawn the schematic, altering a few resistor values and adding ground-reference resistors after the capacitors on the input sides 
(how did input bias not saturate those caps? Are we relying on DC leakage in electrolytics?), I added a 2nd 5V voltage regulator for the microcontroller, lifted the
ground on the microcontroller side, and OF COURSE I added LEDs everywhere, inclduing the ones under the clear-shafted potentiometers. I haven't compared the
character of my version against the original, so who knows if there's any improvement. Probably not, but hey, fun project!

Back in the '80s, Casio released the FV-1 keyboard with four of the MB87186 chips inside. And that's the only place you can get them. The FV-1 and the FV-1M keyboard.
If you have one of these kicking around with a broken logic board or keybed or output whatever, you do have four of these chips that are probably okay. One of the chips I
built up into a module looked pretty rough, water? stains and corroded bent pins, still worked.

For more information on the MB87186 (and from what much of the code
was based on) see http://www.buchty.net/casio/dcf.html

See, this is a dual DCF and DCA. "D" standing for digital, of course, so digitally-controlled filter and amp. ALM developed the code to turn voltages into digital signals, and figured out (based on excellent work linked above) how to get that datastream into the chip so it knows what to do with it. Great work, everybody, hats off.

## Building and flashing the firmware

The most frightning part of this project was getting the code onto the microprocessor. The flash.sh shell script will work with the correct hardware,
but I don't have a Macintosh computer, or a USBTiny programmer. I just have a HP laptop and a bunch of Arduinos flashed with the "Arduino-as-ISP" firmware because I keep
losing them and making new ones. AND I'm not super comofortable with avrdude... so I'll include the .hex file over in the firmware folder and a bit of explanation of the
commands. It may be possible to brick the ATMega644 processor by setting fuses, at least something like that may have happened with one of mine.

> To use the original shell script...
>
> You'll need a basic 'usbtiny' AVR programmer.
>
> You'll need to install avr-gcc and avrdude.
>  
>   /(On mac using brew..)/
>   #+BEGIN_SRC
>   brew tap osx-cross/avr
>   brew install avr-gcc
>   brew install avrdude
>
>   #+END_SRC
>  Type 'make' in the firmare/ directory to compile the source code and
>
> then run './flash.sh' (with your programmer connected) to program the Fizzle.

Okay so here's what worked for me:

I followed the *arduous* steps over [here, which goes through how to create an AVR toolchain](https://tinusaur.com/guides/avr-gcc-toolchain/) and that
was complicated and weird, and the script in `flash.sh` wouldn't work. Mostly because avrdude changed the nanme of programmers, and who knows what else. Anyway,
as of April 2026, here's the command line I used to flash the ATMega644p chip:

`avrdude -V -p atmega644p -c avrisp -P COM5 -b 19200 -F -e -U hfuse:w:0xD8:m -U lfuse:w:0xFF:m -U efuse:w:0x04:m -U flash:w:"PATH TO fz1.hex FILE":a`

`-p atmega644p` means you're programming this particular chip

`-c avrisp` means you're using an Arduion-as-ISP programmer, which are [easy to build](https://docs.arduino.cc/built-in-examples/arduino-isp/ArduinoISP/) with an Arduino Nano

`-P COM5` referrs to which port you're using. Figure this out by looking at your Device Manager, under Ports (COM & LPT) and find the USB-SERIAL CH340 one (at least that's
what it'll be if you're using a cheap Nano clone)

`-b 19200` sets the BAUD rate, probably fine, don't even need it?

The `hfuse` and whatnot, those are fuses that tell the microprocessor which clock to use. The ATMega644 can clock itself, but for this project it must be clocked from the external 20MHz crystal. This might be how I bricked one of my modules, by trying to set fuses and messing an important one up.

The last important part of that command line is what file to flash. You'll find an fz1.hex file elsewhere in this repo, so download it somewhere and delete "PATH TO fz1.hex FILE" and replace it with "C:/Users/NicCage/Downloads/fz1.hex" (what's up Nic? cool movies) or whatever.

Then, plug your Arduino-as-ISP thingie into the header on the module, minding that you get the pins right, plug the Arduino into 
a USB port, open a command prompt window (bash shell if you've been following directions), paste that code into the command line, hit enter, and say "no" when it asks you if you want to re-set the fuse that was just set. Somebody smarter than me can fix whatever's causing that error.

SO anyway, this procedure worked for me, and I've got a functioning FZilter module in my possession.

## License

Code: MIT license.

Hardware: cc-by-sa-3.0

## Guidelines for derivative works

*ALM is a registered trademark.*

The name "ALM" should not be used on any of the derivative works you create from these files.

We kindly ask to do not name any derivatives 'FIZZLE GUTS' but give it a new name. 
