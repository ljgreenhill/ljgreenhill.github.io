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
In our system, we had two separate circuits: one for the mobile robot and one for the stationary Pico. 

The mobile robot has two H-bridge circuits, one to control each motor. One H-bridge circuit was driven by PWM outputs from pins 10 and 11, and the other was driven by PWM outputs from pin 14 and 15. Two 4N35 optoisolators were used in each H-bridge circuit—each one had its anode connected to a 300 Ω resistor and then to a GPIO pin on the RP2040. The anodes were connected to the ground rail. The 4N35’s collector pins were connected to the power rail. Their emitter pins were connected to 10 kΩ resistors and their bases were connected to 1 MΩ resistors, all of which were then connected to ground rail. The L9110 motor driver’s GND was connected to the ground rail. The 4N35’s emitters were connected to the L9110’s IA and IB pins. The L9110’S ground pins were connected to the ground rail, its VCC pins were connected to the power rail, and its OB and OA pins were connected to the positive and negative terminals of the motor, respectively. The capacitor and four 1.5V AA batteries in series (for a total of 6V) were connected in parallel with the motor, i.e. to the power and ground lines. The MCU's GPIO 16 pin for the output audio was connected to a 330 Ω resistor, which was then connected to the 2N3904 NPN transistor’s base. The transistor’s collector was connected to MCU ground and its emitter was connected to the ground lead of the AST-1732MR-R speaker. The power pin of the speaker was connected to a 9V battery. The ground line was connected to the motor ground. The RP2040 was powered by two 1.5 AA batteries in series (for a total of 3V), connected to the VSYS pin. The mobile robot circuit rested on a robot car chassis. 

The stationary Pico was powered by a laptop. The audio socket for the speaker was connected to the Pico, with input connected to GPIO 16 and GND to MCU ground. In both circuits, the microphone was connected in the following manner: GND to MCU ground, VCC to 3V3, and OUT to GPIO 26.

#### Mobile Robot Circuit Diagram

![Mobile Robot Circuit Diagram](https://i.ibb.co/Zxn8n31/test.png)

#### Stationary Pico Circuit Diagram

![Stationary Pico Circuit Diagram](https://i.ibb.co/VNFDCFZ/stationary.png)

| Component | Description | 
| --------- | ------------| 
| RP2040 | This was the microcontroller onto which our programs were loaded. | 
| 4N35 Optoisolators | The 4N35 isolates the RP2040 from the motor, helping protect the microcontroller from voltage spikes that can come off the motor. |
| Capacitors | The 0.1 μF capacitors provide a path to ground to drain any high-frequency noise generated. |
| L9110 H-Bridge Motor Driver | Used to convert a PWM signal to a motor angular velocity through controlling current delivered to the motor. |
| Hobby Gearmotor | Each of the two motors in the mobile robot is connected to a wheel, so the rotation of the motor rotates the wheel. |
| Microphone | The microphones connected to each Pico were used to detect beeps generated by the other Pico. |
| Batteries | Batteries were used to power the mobile robot's motors, speaker, and RP2040. |
| Speakers | The speakers were used to emit the beeps generated by each Pico. |
| Audio Socket | The audio socket is used to connect to the output of the stationary Pico's core in order to hear the audio produced. |
| Robot Car Chassis | The chassis for the mobile robot consisted of a base with wheels. The breadboard with the circuit and the batteries was taped on top of the base, and the two back wheels were controlled by the motors. |
| 2N3904 NPN Transistor | A transistor was used in the mobile robot circuit to amplify the output audio signal before passing it to the speaker. |

## Code Sources
We used the FFT and DMA channel code from this [audio FFT example](https://github.com/vha3/Hunter-Adams-RP2040-Demos/blob/master/Lab_1/Audio_FFT/fft.c).

The motor PWM channel code was inspired by this [PWM demo](https://github.com/vha3/Hunter-Adams-RP2040-Demos/blob/master/Lab_3/PWM_Demo/pwm-demo.c).


