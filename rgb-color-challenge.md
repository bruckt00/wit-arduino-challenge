# Arduino Color Mixer Challenge

Each person will build a simple color-mixing game using three primary-color
LEDs and two push buttons.

The objective is to create six target colors by selecting the correct LED
combination. The Arduino Serial Monitor displays each target color and tells
you whether your submission is correct.

## Materials (Per Person)

| Item | Quantity |
| --- | --- |
| Arduino Uno | 1 |
| USB cable | 1 |
| Breadboard | 1 |
| Red LED | 1 |
| Yellow LED | 1 |
| Blue LED | 1 |
| Push buttons | 2 |
| 220 ohm resistors | 3 |
| Jumper wires | 1 set |
| Laptop with Arduino IDE | 1 |

## Understanding the Color Mixer

The three LEDs represent the primary colors:

- Red
- Blue
- Yellow

The secondary colors are created by combining two primary-color LEDs.

| Target color | LEDs that should be on |
| --- | --- |
| Red | Red |
| Yellow | Yellow |
| Blue | Blue |
| Purple | Red + Blue |
| Orange | Red + Yellow |
| Green | Blue + Yellow |

The six challenges are presented in a random order, so you will not know which
color comes next.

## Pin Assignments

Use these pins exactly:

| Component | Arduino pin |
| --- | --- |
| Red LED | 9 |
| Blue LED | 10 |
| Yellow LED | 11 |
| NEXT button | 4 |
| SUBMIT button | 3 |
| Master ground | Any GND pin |

## Wiring Instructions

### Step 1 – Connect the master ground

Connect one Arduino GND pin to the breadboard's negative or ground rail. Every
component that needs ground connects to this shared rail.

Do not connect the master ground rail to 5V. You do not need a separate
Arduino GND connection for every component.

### Step 2 – Connect the LEDs

For each LED:

- Connect the long leg to its assigned Arduino pin.
- Connect the short leg through a 220 ohm resistor.
- Connect the other leg of the resistor to the ground rail.

### Step 3 – Connect the buttons

Place both push buttons across the center bridge of the breadboard. For each
button:

- Connect one side to its assigned Arduino pin.
- Connect the other side to the ground rail.

The starter program uses the Arduino's internal pull-up resistors, so no
additional button resistors are required.

## Starter Program

Participants should begin with this program. It verifies that all three LEDs
and both buttons are wired correctly before attempting the full challenge.

```cpp
const int RED_LED = 9;
const int BLUE_LED = 10;
const int YELLOW_LED = 11;

const int NEXT_BUTTON = 4;
const int SUBMIT_BUTTON = 3;

int step = 0;

void setup() {
  pinMode(RED_LED, OUTPUT);
  pinMode(BLUE_LED, OUTPUT);
  pinMode(YELLOW_LED, OUTPUT);

  pinMode(NEXT_BUTTON, INPUT_PULLUP);
  pinMode(SUBMIT_BUTTON, INPUT_PULLUP);

  Serial.begin(9600);

  Serial.println("COLOR MIXER TEST");
  Serial.println("-----------------");
  Serial.println("Press NEXT to cycle through");
  Serial.println("the LED combinations.");
  Serial.println("Press SUBMIT to test the button.");
  Serial.println();

  showCombination();
}

void loop() {
  // NEXT button
  if (digitalRead(NEXT_BUTTON) == LOW) {
    step++;

    if (step > 7) {
      step = 0;
    }

    showCombination();

    while (digitalRead(NEXT_BUTTON) == LOW) {
      delay(10);
    }

    delay(200);
  }

  // SUBMIT button
  if (digitalRead(SUBMIT_BUTTON) == LOW) {
    Serial.println("SUBMIT BUTTON PRESSED!");

    while (digitalRead(SUBMIT_BUTTON) == LOW) {
      delay(10);
    }

    delay(200);
  }
}

void showCombination() {
  // Turn all LEDs off
  digitalWrite(RED_LED, LOW);
  digitalWrite(BLUE_LED, LOW);
  digitalWrite(YELLOW_LED, LOW);

  if (step == 1) {
    digitalWrite(RED_LED, HIGH);
  }
  else if (step == 2) {
    digitalWrite(BLUE_LED, HIGH);
  }
  else if (step == 3) {
    digitalWrite(YELLOW_LED, HIGH);
  }
  else if (step == 4) {
    digitalWrite(RED_LED, HIGH);
    digitalWrite(BLUE_LED, HIGH);
  }
  else if (step == 5) {
    digitalWrite(RED_LED, HIGH);
    digitalWrite(YELLOW_LED, HIGH);
  }
  else if (step == 6) {
    digitalWrite(BLUE_LED, HIGH);
    digitalWrite(YELLOW_LED, HIGH);
  }
  else if (step == 7) {
    digitalWrite(RED_LED, HIGH);
    digitalWrite(BLUE_LED, HIGH);
    digitalWrite(YELLOW_LED, HIGH);
  }

  Serial.print("Current combination: ");

  if (step == 0) {
    Serial.println("ALL OFF");
  }
  else if (step == 1) {
    Serial.println("RED");
  }
  else if (step == 2) {
    Serial.println("BLUE");
  }
  else if (step == 3) {
    Serial.println("YELLOW");
  }
  else if (step == 4) {
    Serial.println("RED + BLUE");
  }
  else if (step == 5) {
    Serial.println("RED + YELLOW");
  }
  else if (step == 6) {
    Serial.println("BLUE + YELLOW");
  }
  else if (step == 7) {
    Serial.println("RED + BLUE + YELLOW");
  }
}
```

## Testing the Starter Program

Open the Serial Monitor and press NEXT repeatedly. You should see:

```text
ALL OFF
RED
BLUE
YELLOW
RED + BLUE
RED + YELLOW
BLUE + YELLOW
RED + BLUE + YELLOW
ALL OFF
```

Pressing SUBMIT should display:

```text
SUBMIT BUTTON PRESSED!
```

Once this works, the wiring is ready for the participant challenge.

## Your Challenge

The goal is to correctly create all six target colors. The Arduino randomly
shuffles the challenges:

- Red
- Blue
- Yellow
- Orange
- Purple
- Green

Press NEXT to cycle through the available LED combinations. Press SUBMIT when
you think you have created the requested color. The Serial Monitor will tell
you whether your answer is correct.

You must correctly complete all six challenges.

### Example: Correct Submission

The first challenge might look like this:

```text
==========================
COLOR MIXER CHALLENGE
==========================
CHALLENGE 1 OF 6
CREATE: GREEN
Press NEXT to cycle through the color combinations.
Press SUBMIT when you have created the requested color.
Current combination: ALL OFF
```

Press NEXT until the current combination is BLUE + YELLOW, then press SUBMIT.
The Serial Monitor responds:

```text
===========================
SUBMISSION RECEIVED
===========================
Your combination: BLUE + YELLOW
********************************************
CORRECT!
********************************************
BLUE + YELLOW = GREEN
Challenge complete!
Press NEXT for the next challenge.
```

### Example: Incorrect Submission

If you choose the wrong combination, the Serial Monitor responds:

```text
===========================
SUBMISSION RECEIVED
===========================
Your combination: RED + BLUE
********************************************
INCORRECT
TRY AGAIN!
********************************************
Press NEXT to choose another color combination.
```

## Full Code (Solution)

```cpp
const int RED_LED = 9;
const int BLUE_LED = 10;
const int YELLOW_LED = 11;

const int NEXT_BUTTON = 4;
const int SUBMIT_BUTTON = 3;

const int TOTAL_CHALLENGES = 6;

int challengeSteps[TOTAL_CHALLENGES] = {1, 2, 3, 5, 4, 6};
int currentChallenge = 0;
int step = 0;
bool challengeComplete = false;

void setup() {
  pinMode(RED_LED, OUTPUT);
  pinMode(BLUE_LED, OUTPUT);
  pinMode(YELLOW_LED, OUTPUT);

  pinMode(NEXT_BUTTON, INPUT_PULLUP);
  pinMode(SUBMIT_BUTTON, INPUT_PULLUP);

  Serial.begin(9600);
  randomSeed(analogRead(A0));
  shuffleChallenges();

  printChallengeHeader();
  showCombination();
}

void loop() {
  if (digitalRead(NEXT_BUTTON) == LOW) {
    if (challengeComplete) {
      currentChallenge++;

      if (currentChallenge >= TOTAL_CHALLENGES) {
        Serial.println();
        Serial.println("ALL 6 CHALLENGES COMPLETE!");
        Serial.println("Congratulations!");
      }
      else {
        challengeComplete = false;
        step = 0;
        printChallengeHeader();
        showCombination();
      }
    }
    else {
      step++;

      if (step > 7) {
        step = 0;
      }

      showCombination();
    }

    waitForButtonRelease(NEXT_BUTTON);
  }

  if (digitalRead(SUBMIT_BUTTON) == LOW && !challengeComplete &&
      currentChallenge < TOTAL_CHALLENGES) {
    submitCombination();
    waitForButtonRelease(SUBMIT_BUTTON);
  }
}

void shuffleChallenges() {
  for (int index = TOTAL_CHALLENGES - 1; index > 0; index--) {
    int swapIndex = random(index + 1);
    int temporary = challengeSteps[index];
    challengeSteps[index] = challengeSteps[swapIndex];
    challengeSteps[swapIndex] = temporary;
  }
}

void printChallengeHeader() {
  Serial.println();
  Serial.println("==========================");
  Serial.println("COLOR MIXER CHALLENGE");
  Serial.println("==========================");
  Serial.print("CHALLENGE ");
  Serial.print(currentChallenge + 1);
  Serial.print(" OF ");
  Serial.println(TOTAL_CHALLENGES);
  Serial.print("CREATE: ");
  Serial.println(colorName(challengeSteps[currentChallenge]));
  Serial.println("Press NEXT to cycle through the color combinations.");
  Serial.println("Press SUBMIT when you have created the requested color.");
}

void showCombination() {
  digitalWrite(RED_LED, LOW);
  digitalWrite(BLUE_LED, LOW);
  digitalWrite(YELLOW_LED, LOW);

  if (step == 1) {
    digitalWrite(RED_LED, HIGH);
  }
  else if (step == 2) {
    digitalWrite(BLUE_LED, HIGH);
  }
  else if (step == 3) {
    digitalWrite(YELLOW_LED, HIGH);
  }
  else if (step == 4) {
    digitalWrite(RED_LED, HIGH);
    digitalWrite(BLUE_LED, HIGH);
  }
  else if (step == 5) {
    digitalWrite(RED_LED, HIGH);
    digitalWrite(YELLOW_LED, HIGH);
  }
  else if (step == 6) {
    digitalWrite(BLUE_LED, HIGH);
    digitalWrite(YELLOW_LED, HIGH);
  }
  else if (step == 7) {
    digitalWrite(RED_LED, HIGH);
    digitalWrite(BLUE_LED, HIGH);
    digitalWrite(YELLOW_LED, HIGH);
  }

  Serial.print("Current combination: ");
  Serial.println(combinationName(step));
}

void submitCombination() {
  Serial.println();
  Serial.println("===========================");
  Serial.println("SUBMISSION RECEIVED");
  Serial.println("===========================");
  Serial.print("Your combination: ");
  Serial.println(combinationName(step));
  Serial.println("********************************************");

  if (step == challengeSteps[currentChallenge]) {
    Serial.println("CORRECT!");
    Serial.println("********************************************");
    Serial.print(combinationName(step));
    Serial.print(" = ");
    Serial.println(colorName(challengeSteps[currentChallenge]));
    Serial.println("Challenge complete!");
    Serial.println("Press NEXT for the next challenge.");
    challengeComplete = true;
  }
  else {
    Serial.println("INCORRECT");
    Serial.println("TRY AGAIN!");
    Serial.println("********************************************");
    Serial.println("Press NEXT to choose another color combination.");
  }
}

const char* combinationName(int combination) {
  if (combination == 1) {
    return "RED";
  }
  if (combination == 2) {
    return "BLUE";
  }
  if (combination == 3) {
    return "YELLOW";
  }
  if (combination == 4) {
    return "RED + BLUE";
  }
  if (combination == 5) {
    return "RED + YELLOW";
  }
  if (combination == 6) {
    return "BLUE + YELLOW";
  }
  if (combination == 7) {
    return "RED + BLUE + YELLOW";
  }
  return "ALL OFF";
}

const char* colorName(int combination) {
  if (combination == 1) {
    return "RED";
  }
  if (combination == 2) {
    return "BLUE";
  }
  if (combination == 3) {
    return "YELLOW";
  }
  if (combination == 4) {
    return "PURPLE";
  }
  if (combination == 5) {
    return "ORANGE";
  }
  return "GREEN";
}

void waitForButtonRelease(int buttonPin) {
  while (digitalRead(buttonPin) == LOW) {
    delay(10);
  }

  delay(200);
}
```
