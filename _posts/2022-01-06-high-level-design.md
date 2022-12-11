---
title: "High Level Design"
layout: post
read-more: true
---

## Background Math

The Nyquist Critical Frequency determines the bandwidth of the FFT algorithm. 

It can be calculated using f<sub>c</sub> = 1/2Δt 
where the bandwith ranges from f - f<sub>c</sub> to f + f<sub>c</sub>.

These formulas and the full explanation can be found [here](https://vanhunteradams.com/FFT/FFT.html).

Based on these equations, the fastest frequency that can be detected is half the sample rate.

In our case, the robot emits a frequency of 2500 Hz and the stationary circuit emits a frequency of 5000 Hz. Therefore a sample rate of 10kHz (a max bound of 5kHz) is accepable for the stationary circuit to detect the robot beeps while a sample rate of 20kHz (a max bound of 10kHz) is more suitable for the robot to detect the stationary circuit beeps.


## Rational and Sources
The goal of our project is to mimic whale echolocation.

TODO

## Logical Structure

The code for both the stationary and mobile circuit feature a interrupt service routine for creating beeps that is called at a rate of 100μs, and one main thread which samples audio using a DMA channel and computes the maximum frequency using a FFT algorithm.

## Hardware/Software Tradeoffs

### Detecting Distance

One of the major decisions we made in our design was how to simulate a whale sending out a sound and then hearing a response in order to determine their relative location. In the first iteration of our hardware design, both the stationary circuit and the robot used a speaker powered by the Pico. However, we found that the microphone was unable to detect beeps made by the speaker. We then decided to power the speaker for both circuits with the 9V battery. We also increased the audio sampling rate from 10kHz to 20kHz to account for the 5000 Hz Nyquist limit. This increased the distance the two circuits could be moved away from each other. We found that the mobile robot still sometimes struggled to detect the beeps made by the stationary circuit. Since the stationary circuit did not have to move, we decided to use an audio socket with wall speakers to increase the output of the stationary ciruit.

We also considered using ultrasonic sensors. However, we decided to use sound since we thought that would make it easier to debug (we can hear the interaction between the circuits and did not need to add an additional indicator like a LED) and our familiarity with detecting audible frequencies from the crickets lab.

### TODO
we need at least one more tradeoff
