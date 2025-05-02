
//Code 1 : Sending Text (Transmitter)
// Library: TMRh20/RF24 (https://github.com/nRF24/)

const int inputPin = 2; // Define the digital pin you want to read from
int sensorValue = 0; // Variable to store the read value 
unsigned long startTime = 0;
bool isTiming = false;
bool wasHigh = false;  // Track previous state of input

#include <SPI.h>
#include <RF24.h>
#include <nRF24L01.h>

RF24 radio(9, 8); // CE, CSN
const byte address[10] = "ADDRESS01";

void setup() {
  pinMode(inputPin, INPUT); //Set the pin as an input
  Serial.begin(9600);
  radio.begin();
  radio.openWritingPipe(address);
  radio.setPALevel (RF24_PA_MIN);
  radio.stopListening();
};

void loop() {
  sensorValue = digitalRead(inputPin); // Read the value from the pin
    if (digitalRead(inputPin) == LOW) {
        if (!isTiming) {
            isTiming = true;
            startTime = millis();  // Start timing
        }
        if (millis() - startTime >= 10000) {  // Check if 10 seconds have passed
            char txt[] = "NO MOVEMENT DETECTED";
            radio.write(&txt, sizeof(txt));
            isTiming = false;  // Reset timing to avoid repeated prints
        }
        wasHigh = false;  // Reset wasHigh when input is LOW
    } else {
        if (!wasHigh) {  // Print only once when input first goes HIGH
            char txt[] = "Healthy Activity";
            radio.write(&txt, sizeof(txt));
            wasHigh = true;
        }
        isTiming = false;  // Reset timing if input goes HIGH
    }
}

