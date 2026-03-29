# 🦯 Project: Smart Cane for the Visually Impaired (Arduino Uno)

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=34a853&height=220&section=header&text=SMART%20CANE%20ENGINEERING&fontSize=45&animation=fadeIn" width="100%" />
</p>

## 📖 Introduction & Objectives
This project is an **intelligent electronic cane** designed to assist visually impaired individuals in detecting obstacles. Using ultrasonic technology, the system measures distance in real-time and alerts the user through synchronized auditory and visual signals.

---

## 🛠️ Hardware Architecture

### 📋 Corrected Pin Configuration (Based on Tinkercad Schematic)

<table>
  <tr>
    <th bgcolor="#34a853"><font color="white">Component</font></th>
    <th bgcolor="#34a853"><font color="white">Arduino Port (Pin)</font></th>
    <th bgcolor="#34a853"><font color="white">Technical Role</font></th>
    <th bgcolor="#34a853"><font color="white">Wire Color</font></th>
  </tr>
  <tr>
    <td>📡 <b>HC-SR04 (Trig)</b></td>
    <td><b>PIN 9</b></td>
    <td>Wave Emitter</td>
    <td>Dark Blue</td>
  </tr>
  <tr>
    <td>📡 <b>HC-SR04 (Echo)</b></td>
    <td><b>PIN 10</b></td>
    <td>Wave Receiver</td>
    <td>Yellow</td>
  </tr>
  <tr>
    <td>🔊 <b>Buzzer (+)</b></td>
    <td><b>PIN 11</b></td>
    <td>Auditory Alert</td>
    <td>Dark Red</td>
  </tr>
  <tr>
    <td>🔴 <b>LED (Red/Signal)</b></td>
    <td><b>PIN 13</b></td>
    <td>Visual Alert</td>
    <td>Dark Blue</td>
  </tr>
</table>

---

## 💻 Source Code & Logic Analysis

The "brain" of the cane is programmed to execute 4 distinct logical phases for maximum efficiency.

```cpp
/**
 * 🦯 PROJECT: SMART CANE (3AC Level)
 * Developed by: Rayan_Dev 🇲🇦
 * Corrected Pins based on Technical Schematic
 */

// 1. PIN CONFIGURATION (VERIFIED)
const int TRIG_PIN   = 9; 
const int ECHO_PIN   = 10;  
const int BUZZER_PIN = 11;  // Corrected to Pin 11
const int LED_PIN    = 13; // Corrected to Pin 13

void setup() {
  pinMode(TRIG_PIN, OUTPUT); 
  pinMode(ECHO_PIN, INPUT);  
  pinMode(BUZZER_PIN, OUTPUT);
  pinMode(LED_PIN, OUTPUT);
  Serial.begin(9600); 
}

void loop() {
  // --- STEP 1: EMISSION (TRIGGER) ---
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  // --- STEP 2: RECEPTION (ECHO) ---
  long duration = pulseIn(ECHO_PIN, HIGH);

  // --- STEP 3: DISTANCE CALCULATION ---
  int distance = duration * 0.034 / 2;

  // --- STEP 4: ALERT LOGIC ---
  if (distance > 0 && distance <= 30) {
    digitalWrite(LED_PIN, HIGH); // Danger detected
    tone(BUZZER_PIN, 1000);      // Warning Beep
    delay(100);
    noTone(BUZZER_PIN);
    delay(100);
  } else {
    digitalWrite(LED_PIN, LOW);  // Clear path
    noTone(BUZZER_PIN);
  }
  delay(50); 
}
