// ESP32 Automatic Plant Watering System
// Pin connections follow the ESP32 connection table

#define SOIL_SENSOR 34 // Soil moisture sensor AO
#define GREEN_LED 18   // via 220 ohm resistor
#define YELLOW_LED 19  // via 220 ohm resistor
#define RED_LED 21     // via 220 ohm resistor
#define BUZZER 23      // Buzzer signal (+)
#define RELAY_PIN 22   // Relay module IN

// Most 1-channel relay modules are active-LOW (LOW = relay ON)
#define RELAY_ACTIVE_LOW true

int threshold = 2000; // > 2000 = DRY, <= 2000 = WET

void pumpOn() {
  digitalWrite(RELAY_PIN, RELAY_ACTIVE_LOW ? LOW : HIGH);
}

void pumpOff() {
  digitalWrite(RELAY_PIN, RELAY_ACTIVE_LOW ? HIGH : LOW);
}

void beep(int times, int duration) {
  for (int i = 0; i < times; i++) {
    digitalWrite(BUZZER, HIGH);
    delay(duration);
    digitalWrite(BUZZER, LOW);
    delay(duration);
  }
}

void setup() {
  Serial.begin(115200);
  pinMode(SOIL_SENSOR, INPUT);
  pinMode(GREEN_LED, OUTPUT);
  pinMode(YELLOW_LED, OUTPUT);
  pinMode(RED_LED, OUTPUT);
  pinMode(BUZZER, OUTPUT);
  pinMode(RELAY_PIN, OUTPUT);

  // Everything OFF at start (pump OFF first)
  pumpOff();
  digitalWrite(GREEN_LED, LOW);
  digitalWrite(YELLOW_LED, LOW);
  digitalWrite(RED_LED, LOW);
  digitalWrite(BUZZER, LOW);

  Serial.println("ESP32 Automatic Plant Watering System Initialized");
  Serial.println("----------------------------------------------");
}

void loop() {
  // Yellow LED blinks briefly on every reading (monitoring indicator)
  digitalWrite(YELLOW_LED, HIGH);

  int soilValue = analogRead(SOIL_SENSOR);
  Serial.print("Soil Moisture Value: ");
  Serial.println(soilValue);

  if (soilValue > threshold) {
    // DRY soil -> pump ON
    Serial.println("Soil Status: DRY");
    Serial.println("Pump Status: ON");
    digitalWrite(RED_LED, HIGH);
    digitalWrite(GREEN_LED, LOW);
    pumpOn();
    beep(2, 100); // short double beep as alert
  } else {
    // WET soil -> pump OFF
    Serial.println("Soil Status: WET");
    Serial.println("Pump Status: OFF");
    digitalWrite(GREEN_LED, HIGH);
    digitalWrite(RED_LED, LOW);
    pumpOff();
    digitalWrite(BUZZER, LOW);
  }

  Serial.println("----------------------------------------------");
  delay(200);
  digitalWrite(YELLOW_LED, LOW);
  delay(1600); // total loop cycle ~2 seconds
}
