# Vashista-Hackathon-VH133
VH 133
#include <SPI.h>
#include <nRF24L01.h>
#include <RF24.h>
RF24 radio(9, 10); // CE, CSN

#include <Wire.h> // Include Wire library for I2C communication
#include <Adafruit_Sensor.h>
#include <Adafruit_ADXL345_U.h>
// Initialize the ADXL345 sensor object
Adafruit_ADXL345_Unified accel = Adafruit_ADXL345_Unified(12345);

int forcePin = A0;
int forceValue = 0;
int buzzerPin = 7;

const int mq135Pin = A1;
int mq135Value = 0;
int airQuality = 0;



const byte address[6] = "00014";

void setup() {
  Serial.begin(9600);
  radio.begin();
  radio.setAutoAck(false);
  radio.openWritingPipe(address);
  radio.setPALevel(RF24_PA_MIN);
  radio.stopListening();

  pinMode(forcePin, INPUT);
  pinMode(buzzerPin, OUTPUT);

  if (!accel.begin()) {
    Serial.println("Could not initialize ADXL345 sensor");
    while (1);
  }

  pinMode(mq135Pin, INPUT);

  //dht.begin();
}

void loop() {
  // Read force sensor value
  forceValue = analogRead(forcePin);
  Serial.print("Force Sensor: ");
  Serial.println(forceValue);

  // Read MQ135 gas sensor value
  mq135Value = analogRead(mq135Pin);
  Serial.print("MQ135 Sensor: ");
  Serial.println(mq135Value);
 

  // Read ADXL345 accelerometer values
  sensors_event_t event;
  accel.getEvent(&event);
  Serial.print("X: ");
  Serial.print(event.acceleration.x);
  Serial.print(" m/s^2\tY: ");
  Serial.print(event.acceleration.y);
  Serial.print(" m/s^2\tZ: ");
  Serial.print(event.acceleration.z);
  Serial.println(" m/s^2");

  // Read temperature and humidity using DHT11 sensor
 

  // Turn on buzzer if gas level is above threshold
  if (airQuality > 50) {
    digitalWrite(buzzerPin, HIGH);
  } else {
    digitalWrite(buzzerPin, LOW);
  }

  // Send sensor data to receiver using NRF24L01 wireless module
  radio.write(&forceValue, sizeof(forceValue));
  radio.write(&mq135Value, sizeof(mq135Value));
  radio.write(&airQuality, sizeof(airQuality));
  radio.write(&event.acceleration.x, sizeof(event.acceleration.x));
  radio.write(&event.acceleration.y, sizeof(event.acceleration.y));
  radio.write(&event.acceleration.z, sizeof(event.acceleration.z));


  delay(1000);
}

     
