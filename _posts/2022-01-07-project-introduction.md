---
title: "Project Introduction"
layout: post
read-more: false
---

One sentence "sound bite" that describes your project.
A summary of what you did and why.

Our goal was to simulate whale echolocation, where species send out a series of clicks and intepret the echoes these make when they bounce back from objects. Our project does just that!

In our setup, there are two whales, one represented by a stationary Raspberry Pi Pico microcontroller and one mobile robot also programmed with a Raspberry Pi Pico. Both whales are equipped with a microphone and a speaker. When the moving whale emits clicks, the stationary whale detects the sounds and reponds with its own clicks of another frequency. The mobile whale can use the calculated time from when a click is sent to when a response  is received to determine the distance to the stationary whale and decides which direction to move in, based on our implementation of a state machine to navigate in a grid-like manner towards the stationary whale.