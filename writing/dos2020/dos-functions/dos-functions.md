---
title: "DOS2020: Locations of DOS functions"
author: James Heggie
date: 27/05/2021
keywords: DOS 2020 IBM PC
description: DOS functions move between headers depending on the build system
---

# TL;DR

```
(1) Headers...
#include <dos.h>
#include <conio.h>

(2) Interrupt control
_enable();
_disable();

(3) Calling interrupts
int86(...); & family are all portable

(4) Port I/O
/* Stick to 8 or 16 bit accesses */
outp();
inp();
```

# What functions live where?

There's actually far more portability here than I thought.

## Controlling interrupts

--------------------------------------------------------------------------------
Function/        DJGPP           Borland         Watcom          Microsoft
 structure
---------------- --------------- --------------- --------------- ---------------
enable()/        dos.h (but      dos.h (bios.h   Not present     Not present
disable()        bios.h includes does not
                 this)           include this!)

_enable()/       dos.h           dos.h           i86.h (but      dos.h
_disable()                                       dos.h includes
                                                 this)
--------------------------------------------------------------------------------

## Calling interrupts

--------------------------------------------------------------------------------
Function/        DJGPP           Borland         Watcom          Microsoft
 structure
---------------- --------------- --------------- --------------- ---------------
int86/           dos.h           dos.h           i86.h (included dos.h
int86x                                           via dos.h)

intdos/          dos.h           dos.h           dos.h           dos.h
intdosx

int386/          dos.h           dos.h           i86.h (included Not present
int386x                                          via dos.h)
--------------------------------------------------------------------------------

## Ports

--------------------------------------------------------------------------------
Function/        DJGPP           Borland         Watcom          Microsoft
 structure
---------------- --------------- --------------- --------------- ---------------
inportb()/       pc.h (but       dos.h +         Not present     Not present
outportb()       dos.h includes  conio.h
                 this & thus as
                 does bios.h)

inp()/           As above.       As above.       conio.h         conio.h
outp()

inpd()/          Not present     Not present     conio.h (if     Not present
outpd()                                          \_\_386__
                                                 defined)

inportl()/       pc.h (as above) Not present     Not present     Not present
outportl()
--------------------------------------------------------------------------------

Note: 32-bit port access has no common set of functions. Hopefully this isn't
an issue as DOS-era hardward requiring this is fairly rare I think. Trivial
to implement in NASM if required but not sure is possible to replicate inline
behaviour seen in DJGPP and Watcom.


# Some notes

## Header files

These functions usually live in one of a few headers so there is some degree
of pseudo-standardization...

+ ```dos.h``` - Mappings of int 21h functions
+ ```bios.h``` - Mappings of BIOS functions - e.g. int 13h
+ ```io.h``` - Odd selection of other I/O functions
+ ```conio.h``` - Odd selection of other I/O functions

Some other headers seem to be environment specific...

+ ```pc.h``` (DJGPP) - Although this is included by ```dos.h``` so probably
  doesn't matter that it's independent.
+ ```i86.h``` (Watcom) - Although included by ```dos.h``` so, as above,
  can probably be ignored.