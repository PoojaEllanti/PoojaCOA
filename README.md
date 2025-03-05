# PoojaCOA
Fire Detection System
#define BLYNK_TEMPLATE_ID "TMPL3TgRZSGF4"
#define BLYNK_TEMPLATE_NAME "Fire Detection Alarm"
#defineBLYNK_AUTH_TOKEN "FLwN7W4VqMQKYX8pIRbQ2Q_hAHB4QwTA"

#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <BlynkSimpleEsp8266.h>

#define FLAME_SENSOR_PIN D3
#define BUZZER_PIN D8

char ssid[] = "Arjun";         // Wi-Fi SSID
char pass[] = "12345678";     // Wi-Fi password

LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup() {
  pinMode(FLAME_SENSOR_PIN, INPUT);
  pinMode(BUZZER_PIN, OUTPUT);
  digitalWrite(BUZZER_PIN, LOW);

  Serial.begin(115200);
  lcd.init();
  lcd.backlight();
  lcd.setCursor(0, 0);
  lcd.print("Fire_Sensor_Ready");

  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
  Serial.println("Flame Detection System Initialized");
}

void loop() {
  Blynk.run();

  int flameDetected = digitalRead(FLAME_SENSOR_PIN);

  if (flameDetected == LOW) {
    Serial.println("Flame Detected! Activating Buzzer...");
    digitalWrite(BUZZER_PIN, HIGH);

    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Fire Detected!");
    Blynk.virtualWrite(V0, "Fire Detected!");
    Blynk.logEvent("fire_detected");

  } else {
    digitalWrite(BUZZER_PIN, LOW);

    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("No Fire");
    Blynk.virtualWrite(V0, "No Fire");
  }
  
  delay(200);
}       
