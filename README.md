#include<Servo.h>

#define S0 4
#define S1 5
#define S2 6
#define S3 7
#define sensorOut 8

Servo motor;
int bluefrequency = 0;
int greenfrequency = 0;
int color = 0;
int servoPosition = 0; // Initial servo position

void setup() {
  pinMode(S0, OUTPUT);
  pinMode(S1, OUTPUT);
  pinMode(S2, OUTPUT);
  pinMode(S3, OUTPUT);
  pinMode(sensorOut, INPUT);

  // Setting frequency-scaling to 20%
  digitalWrite(S0, HIGH);
  digitalWrite(S1, LOW);

  motor.attach(9);
  motor.write(servoPosition);

  Serial.begin(96);
}

void loop() {
  // Setting red filtered photodiodes to be read
  digitalWrite(S2, HIGH);
  digitalWrite(S3, LOW);
  bluefrequency = pulseIn(sensorOut, LOW);

  // Setting Green filtered photodiodes to be read
  digitalWrite(S2, HIGH);
  digitalWrite(S3, HIGH);
  greenfrequency = pulseIn(sensorOut, HIGH);

  if (bluefrequency > 35 && bluefrequency < 51) {
    Serial.println("Detecting blue");
    moveServo(50); // Move servo to 50 degrees for red color
  } else if (greenfrequency > 40 && greenfrequency < 70) {
    Serial.println("Detecting green");
    moveServo(150); // Move servo to 150 degrees for green color
  } else {
    Serial.println("No specific color detected");
    moveServo(0); // Move servo to 0 degrees for other colors
  }
}

void moveServo(int position) {
  if (servoPosition < position) {
    for (int i = servoPosition; i <= position; i++) {
      motor.write(i);
      delay(1);
    }

  }
  servoPosition = position;
  delay(1); // Delay for stability
}
