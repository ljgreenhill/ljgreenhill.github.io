---
title: "High Level Design"
layout: post
read-more: false
---

## Background Math

The Nyquist Critical Frequency determines the bandwidth of the FFT algorithm. 

It can be calculated using f<sub>c</sub> = 1/2Δt 
where the bandwith ranges from f - f<sub>c</sub> to f + f<sub>c</sub>.

These formulas and the full explanation can be found [here](https://vanhunteradams.com/FFT/FFT.html).

Based on these equations, the fastest frequency that can be detected is half the sample rate.

In our case, the robot emits a frequency of 2500 Hz and the stationary circuit emits a frequency of 5000 Hz. Therefore a sample rate of 10kHz (a max bound of 5kHz) is accepable for the stationary circuit to detect the robot beeps while a sample rate of 20kHz (a max bound of 10kHz) is more suitable for the robot to detect the stationary circuit beeps.

## Rationale and Sources
The goal of our project is to mimic whale echolocation. Our group was initially interested in music applications of ECE, but we're also big animal lovers and were interested in a further overlap. Through research, we found that Toothed whales, one of 2 species that emits sounds, communicate through high-frequency clicks and perform echolocation to locate other whales but also understand warning sounds to avoid a certain area. Each whale typically has a different pitch and speed between clicks for others to identify each whale. Through further exploration, we found that prior experimentation done with detecting emitted whale clicks around cargo ships prior to movement.
Cargo ship collisions are the top cause of [whale death](https://www.washingtonpost.com/national/health-science/whales-are-facing-a-big-deadly-threat-along-west-coast-massive-container-ships/2019/03/15/cebee6e8-3eb0-11e9-a0d3-1210e58a94cf_story.html), which are increasing as overseas shipping increases. We thought that instead of anticipating for whales to approach the area of concern, maybe we could synthesize warning sounds emitted by whales to warn species to keep their distance.

This future application of our project would minimize whale casualties by simulating a whale sound onboard a cargo ship, repeatedly emitting 'warning clicks' for real whales to register the sound, calculate the location of the synthesized whale, and understand that the sound is a warning to avoid the location of the cargo ship.

## Logical Structure

The code for both the stationary circuit and robot feature a interrupt service routine for creating beeps, and one main thread which samples audio using a DMA channel and computes the maximum frequency using a FFT algorithm. 

The sequence begins when the robot sends a beep. The stationary circuit detects this beep and will respond. 
This exchange happens 3 times. Each time the time between when the robot sent a beep and received the stationary beep is recorded as a time difference. If the robot does not receive a return beep from the stationary circuit within 1 second, it sends another beep. Once 3 time differences have been recorded, a median time difference is calculated. This median time difference is used to determine if the previous median time difference was smaller than the previous calculated median time difference (meaning the two circuits are closer). 

The robot will then move accordingly as described in the Program Details section of Program and Hardware Design.

## Hardware/Software Tradeoffs

### Detecting Distance

One of the major decisions we made in our design was deciding the mechanics of simulating a whale sending out a sound and then detecting a response in order to determine their relative location. In the first iteration of our hardware design, both the stationary circuit and the robot used a speaker powered by the Pico. However, we found that the microphone was unable to detect beeps made by the speaker. We then decided to power the speaker for both circuits with the 9V battery. We also increased the audio sampling rate from 10kHz to 20kHz to account for detecting a 5000 Hz click (which according to the Nyquist Sampling Theorem, our sampling frequency should be greater than 2 * 5000 = 10,000 Hz). This increased the distance the two circuits could be from each other while detecting the others' beeps. We found that the mobile robot still sometimes struggled to detect the beeps made by the stationary circuit. Since the stationary circuit did not have to move, we decided to use an audio socket with wall speakers to increase the output of the stationary ciruit.

We also considered using ultrasonic sensors. However, we decided to use sound since we thought that would make it easier to debug (we can hear the interaction between the circuits and did not need to add an additional indicator like a LED) and our familiarity with detecting audible frequencies from the crickets lab.

### Detecting *Correct* Time Differences
Since our robots must be relatively close to each other due to constraints in speaker volume output, each movement by the robot will resemble a small difference in the time differences before and after the movement, on the scale of fractions of a second. Due to the uncertainty in the time differences and its impact on our software state machine (discussed later), we decided that in each position, the mobile robot waits for 3-back-and-forths, i.e. 3 time difference values, find the median within the three values, and then move according to the state machine. We were able to account for some uncertainty in our hardware with software additions.