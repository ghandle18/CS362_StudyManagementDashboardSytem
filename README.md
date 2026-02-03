# CS362_StudyManagementDashboardSytem
# Smart Personal Study & Lecture Companion

An embedded system designed to support students in both learning and productivity through decentralized Arduino processing, local decision-making, and cloud-connected logging.

This project combines:
- ⏱️ A Pomodoro-based study timer
- 🧠 Local distraction detection
- 🌡️ Smart environmental comfort control
- 📝 AI-assisted lecture transcription
- ☁️ Cloud logging + iPad dashboard display

Each Arduino performs **independent processing** and communicates **semantic state messages**, rather than raw sensor data.

---

# 🔧 System Architecture

## Overview

| Arduino | Role | Core Responsibility |
|----------|------|--------------------|
| Arduino 1 | Distraction Monitor | Detect excessive movement and classify focus state |
| Arduino 2 | Pomodoro Controller | Manage study/break cycle and feedback |
| Arduino 3 | Environment Controller | Monitor temperature and control fan |
| Arduino 4 (Uno R4 WiFi) | Central Hub | Log events, connect to cloud, manage transcription |

---

# 🧠 Decentralized Processing Design

The system is intentionally designed so that:

- Each Arduino processes its own sensor data
- Decisions are made locally
- Only summarized states are transmitted
- The WiFi board acts as a coordinator, not a processor

---

# 🔍 Arduino 1 – Distraction Monitor

### Inputs
- Ultrasonic sensor OR photoresistor
- Optional sound sensor

### Processing
- Samples sensor every 1–2 seconds
- Uses rolling average or smoothing
- Tracks movement/light spikes within 20-second window
- If threshold exceeded → flags distraction

### Output
- Sends `"DISTRACTED"` or `"FOCUSED"` to Arduino 2
- Lights red LED when distracted

### Example Logic
    if movementCount > threshold within 20 seconds:
    distractionState = true

---

# ⏱️ Arduino 2 – Pomodoro Controller

### Inputs
- Pushbutton (start/stop)
- Potentiometer (optional time adjustment)
- Distraction flag from Arduino 1

### Processing
Implements a state machine:

IDLE → STUDY → BREAK → STUDY → ...


### Outputs
- RGB LED:
  - Green = Study
  - Blue = Break
  - Red = Distracted
- Buzzer when session ends
- Sends session events to Arduino 4

---

# 🌡️ Arduino 3 – Environment Monitor

### Inputs
- Thermistor (NTC 10k)

### Processing
- Converts analog value to temperature
- Computes rolling average
- If temperature > threshold (e.g., 28°C):
  - Activates fan motor
  - Sends `"FAN_ON"` event

### Outputs
- Controls servo/DC motor fan
- Logs fan state changes

---

# 🌐 Arduino 4 – Central Hub (Uno R4 WiFi)

### Inputs
- Receives semantic messages from other Arduinos:
  - SESSION_START
  - DISTRACTED
  - FAN_ON
  - SESSION_END

### Responsibilities
- Upload logs to:
  - Google Sheets
  - Firebase
  - Simple web dashboard
- Interface with transcription backend
- Display summary on LCD

---

# 📝 Lecture Transcription Module

### Hardware
- Analog microphone module

### Flow
1. User presses record button
2. Arduino 4 signals external backend
3. Backend (Python + Whisper/OpenAI) transcribes
4. AI summarizes lecture
5. Summary sent back and shown on LCD or dashboard

---

# 🔌 Hardware Components Used

- 4× Arduino boards (1 Uno R4 WiFi + 3 Uno R3)
- 16x2 LCD display
- RGB LED + standard LEDs
- Pushbuttons
- Buzzer
- Ultrasonic sensor OR photoresistor
- Thermistor
- Servo or DC motor (fan)
- Potentiometer
- Microphone sensor
- Breadboard + resistors + jumper wires

---

# 📡 Communication

- I2C or SoftwareSerial between Arduinos
- WiFi via Uno R4 for cloud integration
- Semantic message protocol (not raw data)

Example messages:
  DISTRACTED:1
  SESSION_START
  FAN_ON:29.2
  SESSION_END:FOCUSED


---

# 🎯 Key Innovation

- Fully decentralized embedded intelligence
- Physical productivity monitoring + AI lecture support
- Real-time environmental adaptation
- Multi-microcontroller communication architecture

---

# 🚀 Future Extensions

- Voice-triggered commands
- Multiple-user tracking
- Advanced anomaly detection
- Mobile app companion
- Study analytics dashboard

---

# 👥 Suggested Team Roles

| Member | Focus |
|--------|--------|
| A | Distraction sensor logic |
| B | Pomodoro state machine |
| C | Temperature + fan control |
| D | WiFi + cloud + AI transcription |

---

# 📅 Development Roadmap

1. Calibrate sensors and implement smoothing
2. Build Pomodoro FSM
3. Establish Arduino-to-Arduino communication
4. Integrate WiFi logging
5. Add transcription backend
6. Perform system integration testing

---

# 📌 Summary

This project transforms simple Arduino components into an intelligent, collaborative study assistant that blends embedded systems, environmental awareness, and AI-enhanced learning into one unified platform.
