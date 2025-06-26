# 🔐 IoT Motion-Based Security System
## 📋 Intern Details

- **👨‍💼 Name:** Shaik Javed Ahmed  
- **🎓 Intern ID:** CT04DF594  
- **🏢 Company:** CodTech IT Solutions  
- **🌐 Domain:** Internet of Things  
- **📅 Internship Duration:** 4 Weeks  
- **🧑‍🏫 Mentor:** Neela Santhosh  


This project creates a security system that detects motion using a PIR sensor, captures an image with ESP32-CAM, and sends an alert via Telegram.

## 📦 Components Used
- ESP32-CAM
- PIR Sensor
- FTDI Programmer
- Jumper Wires
- Telegram App

## ⚙️ How It Works
1. PIR sensor detects motion
2. ESP32-CAM captures a photo
3. Sends the image to a Telegram bot in real-time


## circuit diagram
![Image](https://github.com/user-attachments/assets/ff8c8d9b-da15-468a-ac8f-a4525c392290)

## 🖥️ Circuit Connections

| PIR Sensor | ESP32-CAM |
|------------|------------|
| VCC        | 5V         |
| GND        | GND        |
| OUT        | GPIO 13    |

## 🔧 Setup Telegram
1. Create a bot with @BotFather
2. Get bot token and chat ID
3. Add in Arduino code

## 💻 Code

```cpp
// Full code is available in the ino file

