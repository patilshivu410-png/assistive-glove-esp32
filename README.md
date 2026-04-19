-->Assistive Glove using ESP32
-->Overview

This project converts hand gestures into predefined messages using flex sensors and ESP32.

-->Features

* 5 flex sensor integration
* Gesture → message conversion
* Wireless communication using Blynk

-->Components Used

* ESP32
* Flex Sensors (5)
* Resistors
* WiFi (IoT)

-->Code

#define BLYNK_TEMPLATE_ID "YOUR_TEMPLATE_ID"
#define BLYNK_DEVICE_NAME "FlexGlove"
#define BLYNK_AUTH_TOKEN "YOUR_AUTH_TOKEN"

#define BLYNK_PRINT Serial

#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>

char ssid[] = "Your_WiFi_SSID";
char pass[] = "Your_WiFi_Password";

// 5 flex sensors (assign pins based on your wiring)
const int flexPin1 = 34; // Thumb
const int flexPin2 = 35; // Index
const int flexPin3 = 32; // Middle
const int flexPin4 = 33; // Ring
const int flexPin5 = 25; // Little

BlynkTimer timer;

// Threshold (adjust after calibration)
int threshold = 100;

void sendSensorData() {
  int f1 = map(analogRead(flexPin1), 1500, 3000, 0, 180);
  int f2 = map(analogRead(flexPin2), 1500, 3000, 0, 180);
  int f3 = map(analogRead(flexPin3), 1500, 3000, 0, 180);
  int f4 = map(analogRead(flexPin4), 1500, 3000, 0, 180);
  int f5 = map(analogRead(flexPin5), 1500, 3000, 0, 180);

  // Constrain values
  f1 = constrain(f1, 0, 180);
  f2 = constrain(f2, 0, 180);
  f3 = constrain(f3, 0, 180);
  f4 = constrain(f4, 0, 180);
  f5 = constrain(f5, 0, 180);

  // Debug
  Serial.printf("T:%d I:%d M:%d R:%d L:%d\n", f1, f2, f3, f4, f5);

  String message = "";

  // 👉 Single finger gestures (based on your image idea)
  if (f1 > threshold && f2 < threshold && f3 < threshold && f4 < threshold && f5 < threshold) {
    message = "Need water";
  }
  else if (f2 > threshold && f1 < threshold && f3 < threshold && f4 < threshold && f5 < threshold) {
    message = "Feeling hungry";
  }
  else if (f3 > threshold && f1 < threshold && f2 < threshold && f4 < threshold && f5 < threshold) {
    message = "Yes";
  }
  else if (f4 > threshold && f1 < threshold && f2 < threshold && f3 < threshold && f5 < threshold) {
    message = "No";
  }
  else if (f5 > threshold && f1 < threshold && f2 < threshold && f3 < threshold && f4 < threshold) {
    message = "Need washroom";
  }

  // Print message
  if (message != "") {
    Serial.println("Message: " + message);
    Blynk.virtualWrite(V0, message);
  }
}

void setup() {
  Serial.begin(115200);
  Serial.println("5-Finger Assistive Glove Started");

  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);

  timer.setInterval(1000L, sendSensorData);
}

void loop() {
  Blynk.run();
  timer.run();
}
-->Project Images

<img width="1280" height="720" alt="WhatsApp Image 2026-03-15 at 7 41 24 PM (1)" src="https://github.com/user-attachments/assets/4ba10208-f263-4fb6-97b7-546786139ed7" />

<img width="1280" height="720" alt="WhatsApp Image 2026-03-15 at 7 48 26 PM" src="https://github.com/user-attachments/assets/def9473c-fe3a-4a29-a2ce-b0f2be0ea967" />

<img width="720" height="1280" alt="WhatsApp Image 2026-03-15 at 7 41 24 PM" src="https://github.com/user-attachments/assets/6d6e600d-b873-4553-8417-8410c2ff1d0f" />

-->Presentation


[assistive glove ppt.pptx](https://github.com/user-attachments/files/26864150/assistive.glove.ppt.pptx)


-->Certificate

<img width="1252" height="888" alt="Screenshot 2026-04-19 092610" src="https://github.com/user-attachments/assets/9d7bbc69-c982-4972-807b-09ac767eeab7" />




