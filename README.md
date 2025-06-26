# 🔐 IoT Motion-Based Security System

## 🚀 Project Title:
**Build a Security System That Detects Motion, Captures Images, and Sends Alerts to a Mobile App**

---

## 📋 Intern Details

- **👨‍💼 Name:** Shaik Javed Ahmed  
- **🎓 Intern ID:** CT04DF594  
- **🏢 Company:** CodTech IT Solutions  
- **🌐 Domain:** Internet of Things  
- **📅 Internship Duration:** 4 Weeks  
- **🧑‍🏫 Mentor:** Neela Santhosh  

---

## 📌 Project Overview

This IoT-based security system is designed to **detect motion**, **capture images** using an **ESP32-CAM**, and **send real-time alerts to a smartphone** via **Telegram Bot**. It simulates a basic smart surveillance system using Internet of Things concepts.

---

## 🔧 Components Required

| Component         | Quantity |
|------------------|----------|
| ESP32-CAM        | 1        |
| PIR Motion Sensor| 1        |
| FTDI Programmer  | 1        |
| Jumper Wires     | As needed|
| Telegram App     | 1        |
| Internet (Wi-Fi) | 1        |

---

## 🧩 Circuit Connections
![Image](https://github.com/user-attachments/assets/ff8c8d9b-da15-468a-ac8f-a4525c392290)

### PIR Sensor to ESP32-CAM:

| PIR Sensor | ESP32-CAM |
|------------|------------|
| VCC        | 5V         |
| GND        | GND        |
| OUT        | GPIO 13    |

### FTDI Programmer to ESP32-CAM:

| FTDI Pin | ESP32-CAM Pin |
|----------|----------------|
| TX       | U0R (RX)       |
| RX       | U0T (TX)       |
| GND      | GND            |
| 5V       | 5V             |
| IO0      | GND (Only while uploading code) |

---

## ⚙️ Working Principle

- The **PIR sensor** detects motion.
- The **ESP32-CAM** captures an image.
- An alert with the image is sent to the user's **Telegram bot chat**.

---

## 💻 Arduino Code (`iot_security_cam.ino`)

```cpp
#include <WiFi.h>
#include <WiFiClientSecure.h>
#include <UniversalTelegramBot.h>
#include "esp_camera.h"

const char* ssid = "YourWiFi";
const char* password = "YourPassword";

#define BOTtoken "YourTelegramBotToken"
#define CHAT_ID "YourChatID"

WiFiClientSecure client;
UniversalTelegramBot bot(BOTtoken, client);

#define PIR_PIN 13
unsigned long lastSendTime = 0;
const unsigned long interval = 10000; // 10 seconds

void setup() {
  Serial.begin(115200);
  pinMode(PIR_PIN, INPUT);

  camera_config_t config;
  config.ledc_channel = LEDC_CHANNEL_0;
  config.ledc_timer = LEDC_TIMER_0;
  config.pin_d0 = 5;
  config.pin_d1 = 18;
  config.pin_d2 = 19;
  config.pin_d3 = 21;
  config.pin_d4 = 36;
  config.pin_d5 = 39;
  config.pin_d6 = 34;
  config.pin_d7 = 35;
  config.pin_xclk = 0;
  config.pin_pclk = 22;
  config.pin_vsync = 25;
  config.pin_href = 23;
  config.pin_sscb_sda = 26;
  config.pin_sscb_scl = 27;
  config.pin_pwdn = 32;
  config.pin_reset = -1;
  config.xclk_freq_hz = 20000000;
  config.pixel_format = PIXFORMAT_JPEG;
  config.frame_size = FRAMESIZE_VGA;
  config.jpeg_quality = 10;
  config.fb_count = 2;

  if (esp_camera_init(&config) != ESP_OK) {
    Serial.println("Camera init failed");
    return;
  }

  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  client.setInsecure();
  Serial.println("\nWiFi connected");
  bot.sendMessage(CHAT_ID, "Security Bot Activated", "");
}

void loop() {
  if (digitalRead(PIR_PIN) == HIGH && millis() - lastSendTime > interval) {
    sendPhoto();
    lastSendTime = millis();
  }
}

void sendPhoto() {
  camera_fb_t *fb = esp_camera_fb_get();
  if (!fb) {
    Serial.println("Camera capture failed");
    return;
  }

  bot.sendPhotoByBinary(CHAT_ID, "image/jpeg", fb->len, fb->buf, "Intruder.jpg", "");
  esp_camera_fb_return(fb);
  Serial.println("Photo sent to Telegram!");
}


