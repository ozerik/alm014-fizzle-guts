* M4TM FZilter

[[FZilter.png]]

** Introduction

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

** Building and flashing the firmware

The most frightning part of this project was getting the code onto the microprocessor. The flash.sh shell script will work with the correct hardware,
but I don't have a Macintosh computer, or a USBTiny programmer. I just have a HP laptop and a bunch of Arduinos flashed with the "Arduino-as-ISP" firmware because I keep
losing them and making new ones. AND I'm not super comofortable with avrdude... so I'll include the .hex file over in the firmware folder and a bit of explanation of the
commands. It may be possible to brick the ATMega644 processor by setting fuses, at least something like that may have happened with one of mine.

> To use the original shell script...

> You'll need a basic 'usbtiny' AVR programmer.

> You'll need to install avr-gcc and avrdude.
  
   /(On mac using brew..)/
   #+BEGIN_SRC
   brew tap osx-cross/avr
   brew install avr-gcc
   brew install avrdude

   #+END_SRC
  Type 'make' in the firmare/ directory to compile the source code and

then run './flash.sh' (with your programmer connected) to program the
Fizzle.

** License

Code: MIT license.

Hardware: cc-by-sa-3.0

** Guidelines for derivative works

*ALM is a registered trademark.*

The name "ALM" should not be used on any of the derivative works you create from these files.

We kindly ask to do not name any derivatives 'FIZZLE GUTS' but give it a new name. 
