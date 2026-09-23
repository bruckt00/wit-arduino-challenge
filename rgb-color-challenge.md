# Arduino RGB Color Challenge

Each person will build a "color mixer" using an RGB LED and three knobs
(potentiometers). Each knob controls one color:

- Red
- Green
- Blue

The objective is to recreate specific target colors as quickly and accurately
as possible.

Examples:

- Purple
- Orange
- Teal


## Materials (Per Person)

| Item | Quantity |
| --- | --- |
| Arduino Uno | 1 |
| USB cable | 1 |
| Breadboard | 1 |
| Common cathode RGB LED | 1 |
| 220Ω resistors | 3 |
| 10kΩ potentiometers | 3 |
| Jumper wires | 15–20 |
| Laptop with Arduino IDE | 1 |

## Understanding the RGB LED

An RGB LED is essentially three LEDs in one package:

- Red
- Green
- Blue

By changing the brightness of each color, you can create many different colors.

Examples:

| Red | Green | Blue | Color |
| --- | --- | --- | --- |
| 255 | 0 | 0 | Red |
| 0 | 255 | 0 | Green |
| 0 | 0 | 255 | Blue |
| 255 | 255 | 0 | Yellow |
| 255 | 0 | 255 | Purple |
| 0 | 255 | 255 | Teal |
| 255 | 165 | 0 | Orange |
| 255 | 255 | 255 | White |

## Wiring Instructions

### Step 1 – Connect the RGB LED

Most common-cathode RGB LEDs have four legs:

- Longest leg → GND
- Red leg → Arduino Pin 9 through a 220Ω resistor
- Green leg → Arduino Pin 10 through a 220Ω resistor
- Blue leg → Arduino Pin 11 through a 220Ω resistor

Tip: RGB LEDs are not all wired the same internally. Check the LED's pinout
before connecting it, or label the kits in advance.

### Step 2 – Connect the Potentiometers

Each potentiometer has three pins. For each one:

- Left pin → 5V
- Middle pin (wiper) → Analog input
- Right pin → GND

Assign them like this:

| Potentiometer | Arduino Pin |
| --- | --- |
| Red control | A0 |
| Green control | A1 |
| Blue control | A2 |

## Program

The Arduino continuously:

1. Reads each potentiometer.
2. Converts the reading (0–1023) to a PWM value (0–255).
3. Updates the brightness of the corresponding LED color.

## Your Challenge

Below is the starter code with some parts left blank. Try to fill in the
missing pieces (marked with `// TODO: ___`) before scrolling down to the full
solution. Think about:

- Are the LED pins inputs or outputs?
- Which function reads a potentiometer's analog value?
- How do you convert a 0–1023 reading into a 0–255 brightness value?
- Which function sets an LED's brightness using PWM?

```cpp
const int redLED = 9;
const int greenLED = 10;
const int blueLED = 11;

const int redPot = A0;
const int greenPot = A1;
const int bluePot = A2;

void setup() {
  pinMode(redLED, ____);    // TODO: INPUT or OUTPUT?
  pinMode(greenLED, ____);  // TODO: INPUT or OUTPUT?
  pinMode(blueLED, ____);   // TODO: INPUT or OUTPUT?
}

void loop() {
  // TODO: read each potentiometer (0–1023) and map it to 0–255
  int red = map(analogRead(redPot), 0, 1023, 0, ____);
  int green = ____;
  int blue = ____;

  // TODO: send each brightness value to the matching LED pin
  analogWrite(redLED, red);
  analogWrite(greenLED, ____);
  analogWrite(blueLED, ____);
}
```

## Full Code (Solution)

```cpp
const int redLED = 9;
const int greenLED = 10;
const int blueLED = 11;

const int redPot = A0;
const int greenPot = A1;
const int bluePot = A2;

void setup() {
  pinMode(redLED, OUTPUT);
  pinMode(greenLED, OUTPUT);
  pinMode(blueLED, OUTPUT);
}

void loop() {
  int red = map(analogRead(redPot), 0, 1023, 0, 255);
  int green = map(analogRead(greenPot), 0, 1023, 0, 255);
  int blue = map(analogRead(bluePot), 0, 1023, 0, 255);

  analogWrite(redLED, red);
  analogWrite(greenLED, green);
  analogWrite(blueLED, blue);
}
```
