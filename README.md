# HuskyLens Face Recognition Door Lock System (Arduino)

This project uses a **HuskyLens AI camera** and **Arduino** to build a smart face recognition door lock system. When a known face is detected, the door unlocks for a few seconds using a servo motor, then automatically locks again.

## Features

- Uses **HuskyLens in Face Recognition mode**
- Recognizes trained faces with a unique ID
- Unlocks door using a **servo motor** for authorized faces
- Auto-locks after 3 seconds
- Built using Arduino Uno and SoftwareSerial communication

---

## Components Required

| Component          | Quantity | Description                                 |
|--------------------|----------|---------------------------------------------|
| HuskyLens AI Camera| 1        | For face recognition                        |
| Arduino Uno        | 1        | Microcontroller                             |
| Servo Motor        | 1        | Simulates door lock                         |
| Jumper Wires       | Several  | For connections                             |
| Breadboard (optional)| 1      | For organizing components                   |
| USB Cable          | 1        | To power Arduino and upload code            |

---

## Wiring Diagram

### HuskyLens to Arduino (UART mode using SoftwareSerial):

| HuskyLens Pin | Arduino Uno Pin |
|---------------|------------------|
| TX            | Pin 10 (RX)      |
| RX            | Pin 11 (TX)      |
| GND           | GND              |
| VCC           | 5V               |

### Servo Motor:

| Servo Wire | Arduino Pin |
|------------|-------------|
| Signal     | Pin 9       |
| VCC        | 5V          |
| GND        | GND         |

---

## Arduino Code

```cpp
#include "HUSKYLENS.h"
#include <SoftwareSerial.h>
#include <Servo.h>

SoftwareSerial mySerial(10, 11); // RX, TX
HUSKYLENS huskylens;
Servo doorServo;

void setup() {
  Serial.begin(9600);
  mySerial.begin(9600);
  huskylens.begin(mySerial);
  doorServo.attach(9);         // Servo connected to pin 9
  doorServo.write(0);          // Locked position
}

void loop() {
  if (huskylens.request()) {
    if (huskylens.available()) {
      HUSKYLENSResult result = huskylens.read();

      // Check if recognized face has ID 1
      if (result.ID == 1) {
        Serial.println("Authorized face detected!");
        doorServo.write(90);   // Unlock door
        delay(3000);           // Wait 3 seconds
        doorServo.write(0);    // Lock again
      }
    }
  }
  delay(100);
}
