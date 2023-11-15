# Stabilizing-Rockets-with-Gyro--Stabilized-Fins-Thrust-Vectoring-Technique-using-ESP-32
#include <Timer.h>

#include <ESP32Servo.h>
#include <analogWrite.h>
#include <ESP32PWM.h>

#include <Wire.h>
#include <MPU6050_tockn.h>

int servopin1 = 13;   // Servo pin
int servopin2 = 14;
int servopin3 = 12;
int servopin4 = 27;
int servoXPin = 26; // Servo pin for X-axis stabilization
int servoYPin = 25; // Servo pin for Y-axis stabilization

// fins Servo
Servo servo1;
Servo servo2;
Servo servo3;
Servo servo4;
//vectoring servos
Servo servoX;
Servo servoY;

MPU6050 mpu6050(Wire);
long timer = 0;

void setup() {
  Serial.begin(9600);
  Wire.begin();

  mpu6050.begin();
  mpu6050.calcGyroOffsets(true);

  servo1.attach(servopin1);   // attach objects to pins
  servo2.attach(servopin2);
  servo3.attach(servopin3);
  servo4.attach(servopin4);
  servoX.attach(servoXPin);
  servoY.attach(servoYPin);

}

void loop() {
  mpu6050.update();

  Serial.print("Acceleration: ");
  Serial.print(mpu6050.getAccX());
  Serial.print(", ");
  Serial.print(mpu6050.getAccY());
  Serial.print(", ");
  Serial.print(mpu6050.getAccZ());
  Serial.print(" | ");

  Serial.print("Gyroscope: ");
  Serial.print(mpu6050.getGyroX());
  Serial.print(", ");
  Serial.print(mpu6050.getGyroY());
  Serial.print(", ");
  Serial.print(mpu6050.getGyroZ());
  Serial.println();


  mpu6050.update();

  if (millis() - timer > 1000) {
    Serial.print("angleX : ");
    Serial.print(mpu6050.getAngleX());
    Serial.print("  angleY : ");
    Serial.println(mpu6050.getAngleY());

    timer = millis();
  }

  // x-axis / roll
  if (mpu6050.getAngleX() > -1 && mpu6050.getAngleX() < 1) {
    servo2.write(90);   // set to 90 degrees
  }
  else {
    servo2.write(90 - mpu6050.getAngleY()); // unstable angle
  }

  if (mpu6050.getAngleX() == 0) {
    servo1.write(90); // set to 90 degrees
  }
  else {
    servo1.write(90 + mpu6050.getAngleY()); // unstable angle
  }

  // y-axis / pitch
  if (mpu6050.getAngleY() == 0) {
    servo3.write(90); // set to 90 degrees
  }
  else {
    servo3.write(90 + mpu6050.getAngleX()); // unstable angle
  }

  if (mpu6050.getAngleY() == 0) {
    servo4.write(90); // set to 90 degrees
  }
  else 
  {servo4.write(90 - mpu6050.getAngleX());}

  //thrust vectoring
  // Read the gyro sensor values
  float gyroX = mpu6050.getGyroX();
  float gyroY = mpu6050.getGyroY();

  // Convert the gyro sensor values to servo positions
  int servoXPosition = map(gyroY, -90, 90, 0, 180);
  int servoYPosition = map(gyroX, -90, 90, 0, 180);

  // Limit the servo positions within a certain range
  servoXPosition = constrain(servoXPosition, 0, 90);
  servoYPosition = constrain(servoYPosition, 0, 900);

  // Set the servo positions for stabilization
  servoX.write(servoXPosition);
  servoY.write(servoYPosition);
}
