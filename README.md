# README.md - HuskyLens (Simulated) Face Recognition Door Lock System

This project simulates a HuskyLens-based face recognition system using Arduino and a servo motor. Since I don’t have a HuskyLens device yet, I simulated the recognition using a push button in Tinkercad. When the button is pressed, it acts like HuskyLens detected an authorized face and unlocks a servo-powered door for 3 seconds.

## Features

* Simulates face recognition using a push button
* Unlocks door using a servo motor
* Locks again after 3 seconds
* Easy to run in Tinkercad Circuits

## Components Used (Simulated in Tinkercad)

| Component     | Quantity | Description                   |
| ------------- | -------- | ----------------------------- |
| Arduino Uno   | 1        | Main controller               |
| Push Button   | 1        | Simulates HuskyLens trigger   |
| 10kΩ Resistor | 1        | Pull-down resistor for button |
| Servo Motor   | 1        | Simulates door lock mechanism |
| Breadboard    | 1        | For wiring                    |
| Jumper Wires  | Several  | For connections               |

## How It Works

* When the button is pressed, Arduino assumes HuskyLens has detected a face.
* The servo rotates to 90° (unlocked position).
* After 3 seconds, it returns to 0° (locked position).

## Arduino Code

```cpp
#include <Servo.h>

const int buttonPin = 2;
Servo doorServo;

void setup() {
  pinMode(buttonPin, INPUT);
  doorServo.attach(9);
  doorServo.write(0); // Start locked
}

void loop() {
  int buttonState = digitalRead(buttonPin);
  
  if (buttonState == HIGH) {
    doorServo.write(90); // Unlock
    delay(3000);         // Keep unlocked for 3 seconds
    doorServo.write(0);  // Lock
  }
}
```

## Wiring Notes

* Button:

  * One side → 5V
  * Other side → Pin 2 and to GND via a 10kΩ resistor (pull-down)
* Servo Motor:

  * Signal (orange) → Pin 9
  * VCC (red) → 5V
  * GND (brown) → GND

## Project Preview


<img width="1871" height="805" alt="tinkercad_circuit" src="https://github.com/user-attachments/assets/8a5bd531-d1d7-4a60-9ba4-ef171ec07d94" />
