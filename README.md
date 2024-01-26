#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

#define ENA D0
#define IN1 D1
#define IN2 D2
#define IN3 D3
#define IN4 D4
#define ENB D5

bool forward = 0;
bool backward = 0;
bool left = 0;
bool right = 0;
int Speed;

char auth[] = "*******"; //Enter your Blynk application auth token
char ssid[] = "*******"; //Enter your WIFI name
char pass[] = "*******"; //Enter your WIFI passowrd


void setup()
 {
  pinMode(ENA, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENB, OUTPUT);
}
BLYNK_WRITE(V0) {
  forward = param.asInt();
}

BLYNK_WRITE(V1) {
  backward = param.asInt();
}

BLYNK_WRITE(V2) {
  left = param.asInt();
}

BLYNK_WRITE(V3) {
  right = param.asInt();
}

BLYNK_WRITE(V4) {
  Speed = param.asInt();
}
void smartcar() {
  if (forward == 1) {
    Forward();
    Serial.println("Forward");
  } else if (backward == 1) {
    Backward();
    Serial.println("Backward");
  } else if (left == 1) {
    Left();
    Serial.println("Left");
  } else if (right == 1) {
    Right();
    Serial.println("Right");
  } else if (forward == 0 && backward == 0 && left == 0 && right == 0) {
    Stop();
    Serial.println("Stop");
  }
}

void loop()
 {
  Blynk.run();
  smartcar();
}
void Forward() {
  analogWrite(ENA, Speed);
  analogWrite(ENB, Speed);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}
void Backward() {
  analogWrite(ENA, Speed);
  analogWrite(ENB, Speed);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}
void Left() {
  analogWrite(ENA, Speed);
  analogWrite(ENB, Speed);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}
void Right() {
  analogWrite(ENA, Speed);
  analogWrite(ENB, Speed);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}
void Stop() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}
