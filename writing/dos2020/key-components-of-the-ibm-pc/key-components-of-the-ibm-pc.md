---
title: "DOS2020: Key Components of the IBM PC"
author: James Heggie
date: 23/05/2021
keywords: DOS 2020 IBM PC
description: What's in an IBM PC?
---

::: {.technical-notes}
This page is a technical note made by me for me - it is not a freestanding
article. If it's helpful to you, great; if not, sorry.
:::

## PC components

--------------------------------------------------------------------------------
Component  Original chip Significance
---------- ------------- -------------------------------------------------------
CPU        Intel 8088    All future chips are (nearly) completely compatible
                         with this chip.

Interrupt  Intel 8259A   Converts simple electrical IRQs from devices into
controller               interrupt request bus cycles that the CPU understands.

Timer      Intel 8253    Provides the DOS clock (prior to the introduction of
                         the real-time clock chip), refreshed DRAM & drives the
                         PC speaker.

Keyboard   Intel 8042    Converts keyboard signals into CPU i/o data. Also used
controller               for a selection of non-keyboard things, e.g. A20 line
                         control.

Graphics   MGA, CGA,
           EGA, VGA
--------------------------------------------------------------------------------

## Common PC peripherals

--------------------------------------------------------------------------------
Component  Original chip Significance
---------  ------------- -------------------------------------------------------
Sound
blaster

Adlib
--------------------------------------------------------------------------------
