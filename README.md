# IoT-Based-Parking-System
#include <SoftwareSerial.h>
#define BLYNK_TEMPLATE_ID "TMPL3OecLJk6o"
#define BLYNK_TEMPLATE_NAME "IoT Parking System"
#define BLYNK_AUTH_TOKEN "SBLg5lJpGoagcRucuBS0tnwHaHWsFjmA"
#include <BlynkSimpleEsp8266.h>
#include <ESP8266WiFi.h>
char auth[] = "SBLg5lJpGoagcRucuBS0tnwHaHWsFjmA"; // Your Blynk authentication token
char ssid[] = ""; // Your WiFi network SSID
char pass[] = ""; // Your WiFi network password
// Define pins for IR sensors for each parking slot
int irSensorPins[] = {D1, D2, D3, D4, D5, D6};
int numSlots = 6;
// Define virtual pins for Blynk
int virtualPins[] = {V0, V1, V2, V3, V4, V5};
BlynkTimer timer;
void setup() {
Blynk.begin(auth, ssid, pass);
// Initialize IR Sensors
for (int i = 0; i < numSlots; i++) {
pinMode(irSensorPins[i], INPUT);
}
// Timer to check sensor status periodically
timer.setInterval(1000L, checkParkingStatus);
}
void checkParkingStatus() {
for (int i = 0; i < numSlots; i++) {
int isOccupied = digitalRead(irSensorPins[i]);
// Send the status of each parking slot to the corresponding virtual pin
Blynk.virtualWrite(virtualPins[i], isOccupied);
}
}
void loop() {
Blynk.run();
timer.run();
}
