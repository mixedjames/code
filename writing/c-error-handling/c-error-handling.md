---
title: Error handling in C
keywords: C error handling
date: 16/05/2021
author: James Heggie
description: Thoughts on various ways of handling errors in C
---

Some ideas about how to handle errors in C...

Why does this matter? Well, C doesn't force any "one true error handling style" on
you and there are lots of ways to it. There are some definite pros and cons to each!
This is my thoughts about some of the common ones I've used.

### Types of error

Logic errors
: Errors that can be demonstrated/detected ahead of time - e.g. passing NULL where a
  valid pointer is needed; passing an unexpected negative number; aka bugs!

Runtime errors
: Errors that occur in valid code because of a problem at runtime - e.g. a removable
  drive being removed mid-io operation; a network connection disappearing; a hardware
  failure; resource exhaustion



### 