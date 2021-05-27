---
title: "DOS2020: Useful predefined macros"
author: James Heggie
date: 27/05/2021
keywords: DOS 2020 IBM PC
description: Macros for telling apart build environments
---

# TL;DR

## Identifying the compiler

--------------------------------------------------------------------------------
Macro               DJGPP          Borland C       Watcom C       Microsoft C
---------------     -------------- --------------  -------------- --------------
\_\_DJGPP\_\_       Yes

\_\_BORLANDC\_\_                   Yes

\_\_WATCOMC\_\_                                    Yes

\_MSC\_VER                                                        Yes
--------------------------------------------------------------------------------

## Identifying the bitness and memory model

--------------------------------------------------------------------------------
Macro               DJGPP          Borland C       Watcom C       Microsoft C
---------------     -------------- --------------  -------------- --------------
M_I86               No             No              Yes            Yes
                                                   (real mode)    (real mode)

M_I86SM, M_I86MM,   No             No              Yes            Yes
M_I86CM, M_I86LM,                                  (real mode)    (real mode)
M_I86HM

\_\_TINY\_\_ etc.   No             Yes             Yes            No
                                   (real mode)     (real mode)

\_\_i386\_\_        Yes            No              No             No
--------------------------------------------------------------------------------

## Identifying the target environment

--------------------------------------------------------------------------------
Macro               DJGPP          Borland C       Watcom C       Microsoft C
---------------     -------------- --------------  -------------- --------------
\_\_MSDOS\_\_       Yes            Yes             No             No

_MSDOS              No             No              No             Yes
--------------------------------------------------------------------------------

# How to get a list of predefined macros for each system

## DJGPP

I can't find a DJGPP-specific list of predefined macros and what they do but
I can generate a list of macros...

Open an DJGPP command prompt:
```
GCC -E -x c -dM BLANK.C > MACROS.TXT
```
(Note: you need to be in the same directory as BLANK.C - actually it doesn't
 matter what source file you use, but a blank one wont be contaminated by
 and macros introduced by the file itself)

This will create the file ```MACROS.TXT```.

For reference, the flags used are as follows...

+ ```-E``` - Stop compilation after preprocessing
+ ```-x c``` - Set the language to C irrespective of file extension etc.
+ ```-dM``` - Generate a list of ```#define``` directives for all macros defined
  during compilation.

## Watcom

Defined in the *Open Watcom C/C++ User's Guide*:
```C:\WATCOM\MANUALS/CGUIDE.PDF``` page 75 (their numbering) or page 87 (actual
PDF page)

## Borland C 5

Defining in the *Borland C++ Programmer's Guide and Library Reference*:
```C:\BC5\HELP\BCPP.HLP```, search for "Predefined Macros" in the index.

## Microsoft Visual C 1.5

As always, MSVC turns out to be a pain in the ass. I have yet to find a single
list so here's the trail I've followed so far...

### The included (un)help files

I can't figure out how to open the MSVC help system as one unified system
which I think is at the heart of the problem. However, I did manage to find
this:

```C:\MSVC\HELP\MSCOPTS.HLP```, search for "predefined symbols, undefining"

This states that a list of ANSI and Microsoft-specific predefined macros can be
found in the "C Language Reference and the C++ Language Reference". Perhaps
unsurprisingly I can't find this.

### Re-purposing Borland's standard library

BC5's standard library seems to be one they licensed from *Rogue Wave Software*
and it seems to be designed to work on a massive variety of variously capable
systems.

```STDCOMP.H``` is a veritable treasure trove of macro-based configuration
possibilities. Unfortunately this only lead to the discovery of
```_MSC_VER```

### What about Watcom?

Whilst investigating the Watcom compiler I found the ```M_I86``` macro. The
Watcom manual states that this is defined when compiling a 16-bit 80-86
compatible environment. This then lead to the discovery of the other
model-specific macros which MS seems to share with Watcom...

+ ```M_I86SM``` - Small model
+ ```M_I86MM``` - Medium model
+ ```M_I86LM``` - Large model
+ ```M_I86CM``` - Compact model
+ ```M_I86HM``` - Huge model