#include <NoDelay.h>
NoDelay blinkIntervalLow(250);     // Creates a NoDelay variable set to 250ms
NoDelay blinkIntervalHigh(500);    // Creates a NoDelay variable set to 500ms
const int lm35Pin = A0;            // LM35 O/P pin
int ledPin = 13;                   // Arduino onboard LED pin is D13
int ledState = LOW;                // Initially, set LED state to low
void setup() {
  Serial.begin(9600);              // Initialize serial for debugging
  pinMode(ledPin, OUTPUT);          // Set the mode of the onboard LED (D13) to output
}
void loop() {
  int tempAdcValue;                 // Variable for storing the ADC values
  float tempValue;                   // Variable for storing temperature values
  tempAdcValue = analogRead(lm35Pin);   // Read temperature
  tempValue = (tempAdcValue * 5.0) / 10; // Convert ADC value to equivalent voltage and temperature
  Serial.print("Temperature = ");        // Monitor the values on serial monitor
  Serial.print(tempValue);
  Serial.print(" Degree Celsius\n");
  if (tempValue < 30) {
    if (blinkIntervalLow.update()) {
      toggleLED();
    }
  } else if (tempValue > 30) {
    if (blinkIntervalHigh.update()) {
      toggleLED();
    }
  }
}
void toggleLED() {
  // Toggle LED state
  ledState = (ledState == LOW) ? HIGH : LOW;
  // Set the LED with the ledState variable
  digitalWrite(ledPin, ledState);
}
