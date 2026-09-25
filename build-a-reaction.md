# Build a Reaction Game

Build a reaction game where:

1. The Arduino waits a random amount of time.
2. The LED turns on.
3. As soon as the LED lights up, press the button.
4. The Arduino records your reaction time in milliseconds.
5. The fastest group wins!

## Materials (Per Person)

| Item | Quantity |
| --- | --- |
| Arduino Uno | 1 |
| USB cable | 1 |
| Breadboard | 1 |
| LED (any color) | 1 |
| 220Ω resistor | 1 |
| Push button | 1 |
| 10kΩ resistor (pull-down) | 1 |
| Jumper wires | 7–8 |
| Laptop with Arduino IDE | 1 |

## Wiring Instructions

### Step 1 – Connect the LED

- Long leg (positive) → Arduino Pin 8
- Short leg → 220Ω resistor → GND

### Step 2 – Connect the Button

One side of button:

- Arduino Pin 2

Same side:

- 10kΩ resistor to GND

Opposite side:

- 5V

This creates a stable LOW signal until the button is pressed.

## Your Challenge

Below is the starter code with some parts left blank. Try to fill in the
missing pieces (marked with `// TODO: ___`) before scrolling down to the full
solution. Think about:

- Which pin mode does an LED need? Which one does a button need?
- How do you turn the LED **on** vs **off**?
- How do you measure how long something took using `millis()`?

```cpp
const int ledPin = 8;
const int buttonPin = 2;

void setup() {
  pinMode(ledPin, ____);      // TODO: is the LED an INPUT or OUTPUT?
  pinMode(buttonPin, ____);   // TODO: is the button an INPUT or OUTPUT?

  Serial.begin(9600);

  randomSeed(analogRead(A0)); // creates a different random pattern each time
}

void loop() {
  digitalWrite(ledPin, ____); // TODO: start with the LED off (HIGH or LOW?)

  Serial.println("Get Ready...");

  delay(random(2000, 5000));  // wait a random time between 2 and 5 seconds

  digitalWrite(ledPin, ____); // TODO: turn the LED on (HIGH or LOW?)

  unsigned long startTime = millis(); // record the moment the LED turned on

  while (digitalRead(buttonPin) == LOW) {
    // TODO: what should happen while we wait for the button press?
  }

  unsigned long reactionTime = ____ - ____; // TODO: how long did it take?

  digitalWrite(ledPin, LOW);

  Serial.print("Reaction Time: ");
  Serial.print(reactionTime);
  Serial.println(" ms");

  delay(3000);
}
```

## Full Code (Solution)

```cpp
const int ledPin = 8;
const int buttonPin = 2;

void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(buttonPin, INPUT);

  Serial.begin(9600);

  randomSeed(analogRead(A0));
}

void loop() {
  digitalWrite(ledPin, LOW);

  Serial.println("Get Ready...");

  delay(random(2000, 5000));

  digitalWrite(ledPin, HIGH);

  unsigned long startTime = millis();

  while (digitalRead(buttonPin) == LOW) {
  }

  unsigned long reactionTime = millis() - startTime;

  digitalWrite(ledPin, LOW);

  Serial.print("Reaction Time: ");
  Serial.print(reactionTime);
  Serial.println(" ms");

  delay(3000);
}
```
