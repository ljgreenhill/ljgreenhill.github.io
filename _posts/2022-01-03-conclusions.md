---
title: "Conclusions"
layout: post
read-more: false
---

Overall, our design allowed the mobile robot to move to the general location of the stationary Pico. Mostly due to variable turning and time difference calculated, the mobile robot state machine sometimes exited cases too early, causing the mobile robot to fail to find the stationary Pico. We also did not have time to implement different modes for the stationary Pico that would allow it to indicate to the mobile robot to move in the opposite direction.

## Lessons Learned

One of the major flaws in our design was the ability for us to obtain accurate distance measurements from the time difference between the two beeps. We were able to improve the accuracy of this measurement by recording 3 time differences for each position, but the state machine still periodically failed due to an innacurate time difference measurement. This was most likely due to how we sample audio measurements with a DMA channel which we then ran the FFT algorithm on, introducing variable latency into the system.

Additionally, the microphone and speakers we selected did not allow for long range tests. The stationary Pico often failed to detect beeps produced by the mobile robot. A future design could experiment with different speakers or microphones.

Another issue was variance in the robot movements. We relied on turning each motor on for a set period of time. To improve turning accuracy, we could have added an encoder and programmed a PID for each turn.

## Intellectual Property Considerations

We reused code from previous ECE 4760 labs as mentioned in the Program and Hardware Design section.
