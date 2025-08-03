#Smart Robot Control with 4 DC Motors, L293D, Ultrasonic Sensor & Servo Motor
📌 مشروع باستخدام Arduino Uno وTinkercad
📘 وصف المشروع
يهدف هذا المشروع إلى التحكم في أربع محركات DC باستخدام L293D Motor Driver بالإضافة إلى حساس Ultrasonic وServo Motor، وذلك لمحاكاة حركة روبوت ذكي قادر على الاستجابة للعقبات.

🎯 الأهداف
تحريك 4 محركات DC بترتيب معين:

التقدم للأمام لمدة 30 ثانية.

الرجوع للخلف لمدة 60 ثانية.

الدوران يمين ويسار بالتناوب لمدة دقيقة.

استخدام حساس المسافة (Ultrasonic Sensor):

إذا استشعر وجود جسم على بُعد أقل من 10 سم:

تتوقف المحركات.

يقوم السيرفو بالتحريك للمسح يمين ويسار.

تعكس المحركات الاتجاه.
the arduino code

#include <Servo.h>

Servo myservo;

int servopin = A5;
int trigpin = A1;
int echopin = A0;

int speedpin1 = 3;
int dir1 = 5;
int dir2 = 7;

int speedpin2 = 6;
int dir3 = 8;
int dir4 = 10;

int speedpin3 = 9;
int dir5 = 2;
int dir6 = 4;

int speedpin4 = 11;
int dir7 = 12;
int dir8 = 13;

void setup() {
  Serial.begin(9600);
  myservo.attach(servopin);
  myservo.write(90);
  delay(3000);

  pinMode(trigpin, OUTPUT);
  pinMode(echopin, INPUT);

  pinMode(speedpin1, OUTPUT);
  pinMode(speedpin2, OUTPUT);
  pinMode(speedpin3, OUTPUT);
  pinMode(speedpin4, OUTPUT);

  pinMode(dir1, OUTPUT);
  pinMode(dir2, OUTPUT);
  pinMode(dir3, OUTPUT);
  pinMode(dir4, OUTPUT);
  pinMode(dir5, OUTPUT);
  pinMode(dir6, OUTPUT);
  pinMode(dir7, OUTPUT);
  pinMode(dir8, OUTPUT);
}

void loop() {
  int distance = getDistance();

  if (distance <= 10) {
    stopMotors();
    delay(500);

    back();
    delay(1500);

    stopMotors();
    delay(500);

    int rightDist = scanRight();
    int leftDist = scanLeft();

    if (rightDist > leftDist) {
      turnRight();
    } else {
      turnLeft();
    }

    forward();
  } else {
    forward();
  }

  delay(100);
}

int getDistance() {
  digitalWrite(trigpin, LOW);
  delayMicroseconds(5);
  digitalWrite(trigpin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigpin, LOW);

  long duration = pulseIn(echopin, HIGH);
  return duration * 0.034 / 2;
}

void forward() {
  digitalWrite(dir1, HIGH); digitalWrite(dir2, LOW); analogWrite(speedpin1, 255);
  digitalWrite(dir3, LOW);  digitalWrite(dir4, HIGH); analogWrite(speedpin2, 255);
  digitalWrite(dir5, HIGH); digitalWrite(dir6, LOW); analogWrite(speedpin3, 255);
  digitalWrite(dir7, LOW);  digitalWrite(dir8, HIGH); analogWrite(speedpin4, 255);
}

void back() {
  digitalWrite(dir1, LOW); digitalWrite(dir2, HIGH); analogWrite(speedpin1, 255);
  digitalWrite(dir3, HIGH); digitalWrite(dir4, LOW); analogWrite(speedpin2, 255);
  digitalWrite(dir5, LOW); digitalWrite(dir6, HIGH); analogWrite(speedpin3, 255);
  digitalWrite(dir7, HIGH); digitalWrite(dir8, LOW); analogWrite(speedpin4, 255);
}

void stopMotors() {
  digitalWrite(dir1, LOW); digitalWrite(dir2, LOW);
  digitalWrite(dir3, LOW); digitalWrite(dir4, LOW);
  digitalWrite(dir5, LOW); digitalWrite(dir6, LOW);

digitalWrite(dir7, LOW); digitalWrite(dir8, LOW);

  analogWrite(speedpin1, 0);
  analogWrite(speedpin2, 0);
  analogWrite(speedpin3, 0);
  analogWrite(speedpin4, 0);
}

int scanRight() {
  myservo.write(15);
  delay(700);
  int d = getDistance();
  myservo.write(90);
  delay(300);
  return d;
}

int scanLeft() {
  myservo.write(165);
  delay(700);
  int d = getDistance();
  myservo.write(90);
  delay(300);
  return d;
}

void turnRight() {
  digitalWrite(dir1, LOW); digitalWrite(dir2, HIGH); analogWrite(speedpin1, 255);
  digitalWrite(dir3, HIGH); digitalWrite(dir4, LOW); analogWrite(speedpin2, 255);
  digitalWrite(dir5, HIGH); digitalWrite(dir6, LOW); analogWrite(speedpin3, 255);
  digitalWrite(dir7, LOW); digitalWrite(dir8, HIGH); analogWrite(speedpin4, 255);
  delay(1000);
  stopMotors();
}

void turnLeft() {
  digitalWrite(dir1, HIGH); digitalWrite(dir2, LOW); analogWrite(speedpin1, 255);
  digitalWrite(dir3, LOW); digitalWrite(dir4, HIGH); analogWrite(speedpin2, 255);
  digitalWrite(dir5, LOW); digitalWrite(dir6, HIGH); analogWrite(speedpin3, 255);
  digitalWrite(dir7, HIGH); digitalWrite(dir8, LOW); analogWrite(speedpin4, 255);
  delay(1000);
  stopMotors();
}
