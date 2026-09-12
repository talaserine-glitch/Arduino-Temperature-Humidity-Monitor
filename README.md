# Arduino-Temperature-Humidity-Monitor
This project is an Arduino-based Temperature and Humidity Monitoring System using a DHT11 sensor. The readings are shown on an LCD, with LEDs and a buzzer used to indicate different temperature levels.

## Objective
The main objective is to monitor environmental conditions and quickly detect when the temperature exceeds a safe range.

## How it works
The DHT11 sensor measures the temperature and humidity and sends the data to the Arduino Uno.

The Arduino analyzes the temperature and controls the LEDs and buzzer according to the following levels:

• 🟢 0–30°C: Normal temperature → Green LED ON

• 🟡 Above 30°C to 40°C: Moderate temperature → Yellow LED ON

• 🔴 Above 40°C: High temperature → Red LED ON + Buzzer ON

The current temperature and humidity are continuously displayed on the LCD screen.

##  Possible Applications

Similar temperature monitoring and alarm systems can be used in:

• Factories and industrial facilities

• Server rooms and data centers

• Laboratories

• Greenhouses

• Electrical and electronic equipment rooms

• Weather monitoring stations

## Components

• Arduino Uno

• DHT11/DHT22 Temperature and Humidity Sensor 

• 16×2 I2C LCD Display

• LEDS ( green, yellow, red )

• Piezo Speaker / Buzzer

• 220Ω Resistors

• Breadboard

• Jumper Wires

## Circuit Simulation

![Circuit Simulation](circuit-simulation.jpg)


## Arduino Code 

```cpp
#include <DHT.h>
#include <LiquidCrystal_I2C.h>
#include <Wire.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);

#define DHTPIN A3
#define DHTTYPE DHT11

#define GREEN_LED 5
#define YELLOW_LED 6
#define RED_LED 7
#define BUZZER 8

DHT dht(DHTPIN, DHTTYPE);

int h;
int t;

void setup()
{
  Serial.begin(9600);
  dht.begin();

  lcd.init();
  lcd.backlight();

  pinMode(GREEN_LED, OUTPUT);
  pinMode(YELLOW_LED, OUTPUT);
  pinMode(RED_LED, OUTPUT);
  pinMode(BUZZER, OUTPUT);
}

void loop()
{
  h = dht.readHumidity();
  t = dht.readTemperature();

  Serial.print("Humidity: ");
  Serial.print(h);
  Serial.print(" %, Temp: ");
  Serial.print(t);
  Serial.println(" C");

  lcd.setCursor(0, 0);
  lcd.print(" Simple Circuits");

  lcd.setCursor(0, 1);
  lcd.print(" T:");
  lcd.print(t);
  lcd.print("C");

  lcd.setCursor(11, 1);
  lcd.print("H:");
  lcd.print(h);
  lcd.print("%");

  // Turn everything OFF first
  digitalWrite(GREEN_LED, LOW);
  digitalWrite(YELLOW_LED, LOW);
  digitalWrite(RED_LED, LOW);
  digitalWrite(BUZZER, LOW);

  // Temperature conditions
  if (t >= 20 && t < 30)
  {
    digitalWrite(GREEN_LED, HIGH);
  }
  else if (t >= 30 && t < 40)
  {
    digitalWrite(YELLOW_LED, HIGH);
  }
  else if (t >= 40)
  {
    digitalWrite(RED_LED, HIGH);
    digitalWrite(BUZZER, HIGH);
  }

  delay(1000);
 }








