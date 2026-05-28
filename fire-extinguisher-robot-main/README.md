# Fire Extinguisher Car 

An Arduino-based autonomous fire extinguisher robot car that detects fire using IR flame sensors, moves toward the fire source, and activates a water pump to extinguish the fire.

#  Project Overview

This project is designed for basic fire detection and extinguishing in small environments. The robot uses:

* IR Flame Sensors for fire detection
* Servo Motor for nozzle direction
* Water Pump for extinguishing fire
* Motor Driver for car movement
* Arduino UNO as the controller

The robot continuously scans for fire. Once detected:

1. The car moves toward the flame.
2. Stops near the fire.
3. Rotates the servo toward the flame source.
4. Turns ON the water pump.
5. Sprays water until the fire disappears.


#  Components Used

 COMPONENETS        QUANTITY
 -Arduino UNO         1        
 -IR Flame Sensors    3        
 -L298N Motor Driver  1        
 -DC Motors           2        
 -Servo Motor         1        
 -Mini Water Pump     1        
 -Relay Module5v       1        
 -Robot Chassis       1        

  
#  Working Logic

## No Fire Detected

* Car stays idle or searches.
* Pump remains OFF.

## Fire Detected on Left

* Car turns left.
* Servo rotates left.
* Pump activates.

## Fire Detected on Right

* Car turns right.
* Servo rotates right.
* Pump activates.

## Fire Detected in Front

* Car moves forward.
* Stops near flame.
* Servo centers.
* Pump activates.


#  Arduino Code

```cpp
#include <Servo.h>

// Sensors
#define ir_R A0
#define ir_F A1
#define ir_L A2

// Pump (ACTIVE LOW)
#define pump A5
#define PUMP_ON LOW
#define PUMP_OFF HIGH

// Motors
#define IN1 2
#define IN2 3
#define IN3 4
#define IN4 5

Servo myServo;

int s1, s2, s3;
int currentAngle = 90;
int threshold = 500;

void setup() {
  pinMode(ir_R, INPUT);
  pinMode(ir_F, INPUT);
  pinMode(ir_L, INPUT);

  pinMode(pump, OUTPUT);
  digitalWrite(pump, PUMP_OFF);

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  myServo.attach(9);
  myServo.write(90);

  Serial.begin(9600);
}

void loop() {
  s1 = analogRead(ir_R);
  s2 = analogRead(ir_F);
  s3 = analogRead(ir_L);

  Serial.print("Right: ");
  Serial.print(s1);
  Serial.print(" Front: ");
  Serial.print(s2);
  Serial.print(" Left: ");
  Serial.println(s3);

  if (s2 < threshold) {
    forward();
    delay(500);
    stopMotors();

    myServo.write(90);
    digitalWrite(pump, PUMP_ON);
  }
  else if (s1 < threshold) {
    right();
    delay(400);
    stopMotors();

    myServo.write(40);
    digitalWrite(pump, PUMP_ON);
  }
  else if (s3 < threshold) {
    left();
    delay(400);
    stopMotors();

    myServo.write(140);
    digitalWrite(pump, PUMP_ON);
  }
  else {
    stopMotors();
    digitalWrite(pump, PUMP_OFF);
    myServo.write(90);
  }
}

void forward() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void backward() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void left() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void right() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void stopMotors() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}

#  Features

* Autonomous fire detection
* Automatic water spraying
* Direction-based fire tracking
* Servo-controlled nozzle
* Low-cost embedded project


#  Future Improvements

* Add Bluetooth/WiFi control
* Add smoke sensor
* Add camera module
* ESP32 integration
* Mobile app control
* Voice alerts

---

## Project

BVOC (AI & Robotics)
