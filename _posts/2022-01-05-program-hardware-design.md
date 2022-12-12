---
title: "Program and Hardware Design"
layout: post
read-more: false
---

## Program Details

### Making Beeps

Frequency of beeps is controlled by the rate at which the service routine is called. For the stationary circuit, the interrupt is called every 100μs. Since this is the amount of time the speaker will be on or off, 1 / 2 * 100μs is the frequency of the beep (in this case, 5000 Hz). The interrupt is called every 200μs, or 2500 Hz.

The main thread indicates that a beep should be made by flipping a boolean value. Once this boolean value is true, the service routine uses the following logic:

![Beep State Machine](https://i.ibb.co/WcW4b9T/final-state-machine-Page-2-drawio.png)

In the diagram, if the interrupt service routine is called every 100μs, time is increased every 100μs. Therefore CLICK_DURATION of 750 would result in the speaker being on for 75000μs, or 0.075s. 

### Detecting Beeps

The main loop is constantly sampling audio into a DMA channel. Once enough samples are acquired, the FFT algorithm is run on the samples to calculate max frequency. If the max frequency detected is ±100 Hz of the expected frequency, a beep is acknowledged as received.

### Moving

The mobile robot uses two PWM channels (A and B) to signal to the H-Bridge with what direction and power each motor should be turning. The following table shows the settings we used for duty cycle:

| Direction | Right Motor Channel A | Right Motor Channel B | Left Motor Channel A | Left Motor Channel B |
| --------- | --------------------- | --------------------- | -------------------- | -------------------- |
| Forward   | 0                     | 5000                  | 0                    | 5000                 |
| Backward  | -5000                 | 0                     | -5000                | 0                    |
| Left      | 0                     | 0                     | 0                    | 5000                 |
| Right     | 0                     | 5000                  | 0                    | 0                    |

The wrap value (maximum counter value) was set to 5000 and the clock division (value to divide the counter value by) was set to 25.

In the following diagram, the blue dot is the starting location of the mobile robot, and the orange dot is the stationary Pico. Every time a new median time difference is calculated (three beeps have been sent and received) the thread running on core 0 evaluates if the state machine should switch cases. 

![Moving State Machine](https://i.ibb.co/pnMGG9B/final-state-machine-Page-1-drawio.png)

## Hardware Details
TODO: Sarika

## Code Sources
We used the FFT and DMA channel code from this [audio FFT example](https://github.com/vha3/Hunter-Adams-RP2040-Demos/blob/master/Lab_1/Audio_FFT/fft.c).

The motor PWM channel code was inspired by this [PWM demo](https://github.com/vha3/Hunter-Adams-RP2040-Demos/blob/master/Lab_3/PWM_Demo/pwm-demo.c).


