# Arduino Smart Parking Radar System

This project features a sophisticated **Ultrasonic Parking Assistant** designed to provide real-time distance feedback through visual (LEDs), auditory (Buzzer), and textual (I2C LCD) interfaces. It acts as a high-precision proximity alert system for automotive safety applications.

## Hardware Components
* **Arduino Uno**
* **HC-SR04** Ultrasonic Sensor
* **I2C 16x2 LCD Display**
* **Buzzer**
* **3x LEDs** (Blue, Yellow, Red)
* **330 Ohm Resistors** (for LED current limiting on the cathode side)
* **Jumper Wires & Breadboard**

## Code

         #include <Wire.h>
         #include <LiquidCrystal_I2C.h>

         // LCD ekran adresi
         #define LCD_ADDRESS 0x27

         // LCD ekran sütun ve satır sayısı
         #define LCD_COLUMNS 16
         #define LCD_ROWS 2

         // Pin tanımlamaları
         const int trigPin = 3;
         const int echoPin = 4;
         const int ledPin1 = 8;
         const int ledPin2 = 9;
         const int ledPin3 = 10;
         const int buzzerPin = 6;

         // LCD ekranı başlatma
         LiquidCrystal_I2C lcd(LCD_ADDRESS, LCD_COLUMNS, LCD_ROWS);

         void setup() {
            // LCD ekran başlatma
            lcd.init();
            lcd.backlight();

            // Pin modları ayarlama
            pinMode(trigPin, OUTPUT);
            pinMode(echoPin, INPUT);
            pinMode(ledPin1, OUTPUT);
            pinMode(ledPin2, OUTPUT);
            pinMode(ledPin3, OUTPUT);
            pinMode(buzzerPin, OUTPUT);

            // İletişim hızını ayarlama
            Serial.begin(9600);
           }

         void loop() {
            // Mesafe ölçümü için ultrasonik sensör
            digitalWrite(trigPin, LOW);
            delayMicroseconds(2);
            digitalWrite(trigPin, HIGH);
            delayMicroseconds(10);
            digitalWrite(trigPin, LOW);

            long duration = pulseIn(echoPin, HIGH);
            int distance = duration * 0.034 / 2;

            // Mesafeyi ekrana yazdırma
           
            lcd.setCursor(0, 0);
            lcd.print("Mesafe: ");
            lcd.print(distance);
            lcd.print(" cm");

            // "IBRAHIM YAVUZ" yazdırma
            lcd.setCursor(0, 1);
            lcd.print("IBRAHIM YAVUZ");

            // LED kontrolü
            if (distance <= 15) {
                  // 15 cm'den yakın
                  digitalWrite(ledPin1, HIGH);
                  digitalWrite(ledPin2, LOW);
                  digitalWrite(ledPin3, LOW);
            } else if (distance <= 25) {
                  // 15-25 cm arası
                  digitalWrite(ledPin1, LOW);
                  digitalWrite(ledPin2, HIGH);
                  digitalWrite(ledPin3, LOW);
            } else if (distance <= 35) {
                  // 25-35 cm arası
                  digitalWrite(ledPin1, LOW);
                  digitalWrite(ledPin2, LOW);
                  digitalWrite(ledPin3, HIGH);
            } else {
                  // 35 cm'den uzak
                  digitalWrite(ledPin1, LOW);
                  digitalWrite(ledPin2, LOW);
                  digitalWrite(ledPin3, LOW);
            }

            // Buzzer kontrolü
            if (distance <= 8) {
                  // 8 cm'den yakın
                  tone(buzzerPin, 1000); // 1000 Hz'de bip sesi
                  delay(30); // 30 ms bekle
            } else if (distance <= 15) {
                  // 8-15 cm arası
                  tone(buzzerPin, 1000); // 1000 Hz'de bip sesi
                  delay(100); // 100 ms bekle
                  noTone(buzzerPin); // Buzzeri kapat
                  delay(100); // 100 ms bekle
            } else if (distance <= 25) {
                  // 15-25 cm arası
                  tone(buzzerPin, 1000); // 1000 Hz'de bip sesi
                  delay(200); // 200 ms bekle
                  noTone(buzzerPin); // Buzzeri kapat
                  delay(200); // 200 ms bekle
            } else if (distance <= 35) {
                  // 25-35 cm arası
                  tone(buzzerPin, 1000); // 1000 Hz'de bip sesi
                  delay(300); // 300 ms bekle
                  noTone(buzzerPin); // Buzzeri kapat
                  delay(300); // 300 ms bekle
            }

            delay(100); // Yenileme aralığı
          }
