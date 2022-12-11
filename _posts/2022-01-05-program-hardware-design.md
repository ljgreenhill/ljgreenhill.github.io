---
title: "Program and Hardware Design"
layout: post
read-more: true
---

# Program Details

TODO: Lauren

In the following diagram, the blue dot is the starting location of the mobile robot, and the orange dot is the stationary Pico. Every time a new median time difference is calculated (three beeps have been sent and received) the thread running on core 0 evaluates if the state machine should switch cases. 


![Moving State Machine](https://i.ibb.co/pnMGG9B/final-state-machine-Page-1-drawio.png)

# Hardware Details
TODO: Sarika

# Code Sources
We used the FFT and DMA channel code from this [audio FFT example](https://github.com/vha3/Hunter-Adams-RP2040-Demos/blob/master/Lab_1/Audio_FFT/fft.c).

The motor PWM channel code was inspired by this [PWM demo](https://github.com/vha3/Hunter-Adams-RP2040-Demos/blob/master/Lab_3/PWM_Demo/pwm-demo.c).


