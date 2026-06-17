#include <ESP32Servo.h>

// Finger Servos
Servo thumbServo;
Servo indexServo;
Servo middleServo;
Servo ringServo;
Servo pinkyServo;

// Pins
const int emgPin = 34;

const int thumbPin  = 18;
const int indexPin  = 19;
const int middlePin = 21;
const int ringPin   = 22;
const int pinkyPin  = 23;

// Finger Positions
const int OPEN_POS  = 20;
const int CLOSE_POS = 120;

const int THRESHOLD = 1800;
const int NUM_SAMPLES = 10;

int readEMG()
{
  long sum = 0;

  for(int i = 0; i < NUM_SAMPLES; i++)
  {
    sum += analogRead(emgPin);
    delay(2);
  }

  return sum / NUM_SAMPLES;
}

void openHand()
{
  thumbServo.write(OPEN_POS);
  indexServo.write(OPEN_POS);
  middleServo.write(OPEN_POS);
  ringServo.write(OPEN_POS);
  pinkyServo.write(OPEN_POS);
}

void closeHand()
{
  thumbServo.write(CLOSE_POS);
  indexServo.write(CLOSE_POS);
  middleServo.write(CLOSE_POS);
  ringServo.write(CLOSE_POS);
  pinkyServo.write(CLOSE_POS);
}

void setup()
{
  Serial.begin(115200);

  thumbServo.attach(thumbPin);
  indexServo.attach(indexPin);
  middleServo.attach(middlePin);
  ringServo.attach(ringPin);
  pinkyServo.attach(pinkyPin);

  openHand();

  Serial.println("Bionic Arm Ready");
}

void loop()
{
  int emgValue = readEMG();

  Serial.print("EMG: ");
  Serial.println(emgValue);

  if(emgValue > THRESHOLD)
  {
    closeHand();
    Serial.println("Hand Closed");
  }
  else
  {
    openHand();
    Serial.println("Hand Open");
  }

  delay(50);
}
