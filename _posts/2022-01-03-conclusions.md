---
title: "Conclusions"
layout: post
read-more: false
---

Overall, our design allowed the mobile robot to move to the general location of the stationary Pico. Mostly due to variable turning and time difference calculated, the mobile robot state machine sometimes exited cases too early, causing the mobile robot to fail to find the stationary Pico. With additional time to work on the project, we would implement different moves for the stationary Pico that would allow it to indicate to the mobile robot to move in the opposite direction, seen in the wild when whale species emit warning clicks to avoid a certain area.

## Lessons Learned

One of the major flaws in our design was the ability for us to obtain accurate distance measurements from the time difference between the two beeps. We were able to improve the accuracy of this measurement by recording 3 time differences for each position, but the state machine still periodically failed due to an innacurate time difference measurement. This was most likely due to how we sample audio measurements with a DMA channel which we then ran the FFT algorithm on, introducing variable latency into the system.

Additionally, the microphone and speakers we selected did not allow for long range tests. The stationary Pico often failed to detect beeps produced by the mobile robot. A future design could experiment with different speakers or microphones.

Another issue was variance in the robot movements. We relied on turning each motor on for a set period of time to move in a certain direction for a certain approximate distance. To improve turning accuracy, we could have implemented motor encoders and PID  control for each turn and for moving straight.

## Intellectual Property Considerations

We reused code from previous ECE 4760 labs as mentioned in the Program and Hardware Design section.
