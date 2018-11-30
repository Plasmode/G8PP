# G8PP, Generic 8-bit Processors Prototype board
## Introduction
G8PP is a prototype board for 8-bit processors.  The target processors are Z80, 8085, 680x, 6502, as well as 16-bit processors with 8-bit bus such as 68008 and 32008.  G8PP is a RC2014 bus compatible module to take advantage of various I/O modules of the RC2014 family.
## Features
* 512K battery-backed RAM
* Altera EPM7128 CPLD
* Prototype area supporting 48-pin, 600mil wide DIP
## Theory of Operation
G8PP has two modes of operation as determined by the Bootstrap jumper at power up (see picture below for location of bootstrapjumper). 
### UART Bootstrap
At power up the target processor is held in idle state until 255 bytes of instruction is uploaded to UART (115200 baud, N81, no handshake) which is then copied to memory starting from location 0x0. Afterward the CPU starts execution from 0x0.  This is the mode to load programs into the non-volatile RAM prior to booting via the RAM bootstrap mode.
### RAM Bootstrap
This is normal mode of operation. At powerup, the target processor fetches program from physical location 0 which contains the bootstrap monitor. The bootstrap monitor write protect physical page 0 where bootstrap monitor resides. It then makes a copy of itself to 0xB000-0xBFFF and jump to it. Lastly the physical page 0 is replaced with logical page 0 which mapped to different physical page. This way the physical page 0 containing the bootstrap monitor is write protected and moved out of way. The application program will have unfettered access to all 64K space of the target processor.
