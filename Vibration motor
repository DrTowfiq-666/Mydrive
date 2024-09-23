#define vibOutPin 2
#define voltagePin A0
#define VCC2 8

unsigned long previousMillis = 0;
unsigned long vibrateInterval = 2000; // Time interval for vibration (ON duration)
unsigned long stopInterval = 600;     // Time interval for stopping (OFF duration)
bool isVibrating = false;             // Track if the motor is currently vibrating
bool vibrationCycleStarted = false;   // Track if the vibration cycle has started

void setup() {
  pinMode(vibOutPin, OUTPUT);
  pinMode(VCC2, OUTPUT);
  digitalWrite(VCC2, HIGH); // 5V for potentiometer
  Serial.begin(9600);
  Serial.println("Robojax.com");
  Serial.println("Vibration motor test");
}

void loop() {
  // Read the potentiometer value
  int potValue = analogRead(voltagePin);
  float voltage = potValue * (5.0 / 1023.0);
  
  Serial.print("Voltage: ");
  Serial.print(voltage, 2);
  Serial.println("V");

  // If voltage is greater than 3.8V, start the vibration cycle
  if (voltage > 3.8) {
    vibrate();
  } else {
    stop();
  }

  // Delay for 200ms between readings (this can be adjusted or removed if needed)
  delay(200);
}

// Non-blocking vibration function using millis()
void vibrate() {
  unsigned long currentMillis = millis();

  // Start the vibration cycle if it's not already running
  if (!vibrationCycleStarted) {
    digitalWrite(vibOutPin, HIGH);
    previousMillis = currentMillis;
    isVibrating = true;
    vibrationCycleStarted = true;
  }

  // Check if the motor needs to be turned OFF or ON based on the interval
  if (isVibrating && (currentMillis - previousMillis >= vibrateInterval)) {
    digitalWrite(vibOutPin, LOW);  // Turn off motor
    previousMillis = currentMillis;
    isVibrating = false;
  } else if (!isVibrating && (currentMillis - previousMillis >= stopInterval)) {
    digitalWrite(vibOutPin, HIGH); // Turn on motor
    previousMillis = currentMillis;
    isVibrating = true;
  }
}

// Stops the vibration motor
void stop() {
  digitalWrite(vibOutPin, LOW);
  vibrationCycleStarted = false; // Reset the cycle when stopping
}
