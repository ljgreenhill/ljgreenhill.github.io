---
title: "High Level Design"
layout: post
read-more: true
---

## Background Math

The Nyquist Critical Frequency determines the bandwidth of the FFT algorithm. 

It can be calculated using f<sub>c</sub> = $$ { 1 \over 2Δt} $$
where the bandwith ranges from f - f<sub>c</sub> to f + f<sub>c</sub>.

Please see the full explanation [here](https://vanhunteradams.com/FFT/FFT.html).

## Rational and Sources
The goal of our project is to mimic whale echolocation.

## Logical Structure

The code for both the stationary and mobile circuitd


## Hardware/Software Tradeoffs

### Detecting Distance

One of the major decisions we made in our design was how to simulate a whale sending out a sound and then hearing a response in order to determine their relative location. In the first iteration of our hardware design, both the stationary circuit and the robot used a speaker powered by the Pico. However, we found that the microphone was unable to detect beeps made by the speaker. We then decided to power the speaker for both circuits with the 9V battery. We also increased the audio sampling rate from 10kHz to 20kHz to account for the 5000 Hz Nyquist limit. This increased the distance the two circuits could be moved away from each other. We found that the mobile robot still sometimes struggled to detect the beeps made by the stationary circuit. Since the stationary circuit did not have to move, we decided to use an audio socket with wall speakers to increase the output of the stationary ciruit.

We also considered using ultrasonic sensors. However, we decided to use sound 









