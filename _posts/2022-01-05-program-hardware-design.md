---
title: "Program and Hardware Design"
layout: post
read-more: true
---

# Program Details

In this diagram, the blue dot is the starting location of the mobile robot, and the orange dot is the stationary Pico. Every time a new median time difference is calculated (three beeps have been sent and received) the thread running on core 0 evaluates if the state machine should switch cases. 

![Moving State Machine](https://i.ibb.co/CtjBDr1/moving-state-machine.png)

# Hardware Details
@Sarika

# Code Sources
Some sections of our code were take directly from provided sample code.


