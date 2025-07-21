# 🔌 Latching Power Switch Circuit with Auto Power Off (Tinkercad Project)

## 🧠 Overview
This project simulates a **latching power switch** using an Arduino Uno in **Tinkercad Circuits**. A pushbutton is used to toggle an LED on or off. Once turned on, the LED stays on until the button is pressed again **or** automatically turns off after **10 seconds**.

🔗 [View the Circuit on Tinkercad](https://www.tinkercad.com/things/cleB2VOkiuc-latching-power-circuit?sharecode=nLtXsAfqz7PkTn1E9r-YI3SxcFKH4llwjseQVkV0ce0)

---

## 🛠 Components Used
- Arduino Uno R3
- LED
- 220Ω Resistor (for LED)
- Pushbutton
- 10kΩ Resistor (pull-down)
- Jumper wires

---

## ⚡ Circuit Connections

### 🔘 Pushbutton
- One leg → GND  
- Same leg → 10kΩ resistor → GND (acts as pull-down)  
- Other leg → Digital Pin **2** on Arduino  

### 💡 LED
- Anode (long leg) → Digital Pin **13** through 220Ω resistor  
- Cathode (short leg) → GND  

---

## 🧾 Arduino Code

```cpp
const int buttonPin = 2;     // Pushbutton pin
const int ledPin = 13;       // LED pin

bool isOn = false;
unsigned long startTime = 0;
const unsigned long autoOffTime = 10000; // 10 seconds

void setup() {
  pinMode(buttonPin, INPUT);
  pinMode(ledPin, OUTPUT);
  digitalWrite(ledPin, LOW);
}

void loop() {
  if (digitalRead(buttonPin) == HIGH) {
    delay(200); // debounce
    isOn = !isOn;
    digitalWrite(ledPin, isOn ? HIGH : LOW);

    if (isOn) {
      startTime = millis();
    }

    // Wait for button release
    while (digitalRead(buttonPin) == HIGH);
  }

  // Auto turn off after 10 seconds
  if (isOn && millis() - startTime >= autoOffTime) {
    isOn = false;
    digitalWrite(ledPin, LOW);
  }
}
