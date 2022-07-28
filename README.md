#include <SoftwareSerial.h>
int motorRightA = 8;    //Right Motor-clockwise
int motorRightB = 9;   //Right Motor-anticlockwise
int motorLeftA = 11;   //Left Motor-clockwise
int motorLeftB = 10;   //Left Motor-anticlockwise
int trigPin1 = 12;     // Trig Pin
int echoPin1 = 13;     // Echo Pin
int light = 5;
long duration1;
int distance1;
char bt = 0;            //Bluetooth Control
int trigPin2 = 7;       // Trig Pin
int echoPin2 = 6;       // Echo Pin
long duration2;
int distance2;
int buzzer = 4;
int pushButton = 3;
String voice;
int TxD = TxD;
int RxD = RxD;
SoftwareSerial bluetooth(TxD, RxD);

void setup()
{
  pinMode(motorRightA, OUTPUT);
  pinMode(motorRightB, OUTPUT);
  pinMode(motorLeftA, OUTPUT);
  pinMode(motorLeftB, OUTPUT);
  pinMode(trigPin1, OUTPUT);
  pinMode(echoPin1, INPUT);
  pinMode(trigPin2, OUTPUT);
  pinMode(echoPin2, INPUT);
  pinMode(light, OUTPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(pushButton, INPUT_PULLUP);
  Serial.begin(9600);
  bluetooth.begin(9600);
}
void loop()
{
   //Light On Off
  

  //  Right
  digitalWrite(trigPin1, LOW);
  delayMicroseconds(2);
  // Sets the trigPin on HIGH state for 10 micro seconds
  digitalWrite(trigPin1, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin1, LOW);
  // Reads the echoPin, returns the sound wave travel time in microseconds
  duration1 = pulseIn(echoPin1, HIGH);
  // Calculating the distance
  distance1 = duration1 * 0.1/2;
  // Prints the distance on the Serial Monitor
  Serial.print("Distance1: ");
  Serial.println(distance1);

  // Left 
  digitalWrite(trigPin2, LOW);
  delayMicroseconds(2);
  // Sets the trigPin on HIGH state for 10 micro seconds
  digitalWrite(trigPin2, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin2, LOW);
  // Reads the echoPin, returns the sound wave travel time in microseconds
  duration2 = pulseIn(echoPin2, HIGH);
  // Calculating the distance
  distance2 = duration2 *0.1/2;
  // Prints the distance on the Serial Monitor
  Serial.print("Distance2: ");
  Serial.println(distance2);

  if (distance1 <= 25 || distance2 <= 25) {
    //Stop Wheel Chair
    digitalWrite(motorRightA, LOW);
    digitalWrite(motorRightB, LOW);
    digitalWrite(motorLeftA, LOW);
    digitalWrite(motorLeftB, LOW);
  }   
  control();
}
 void control()
 {
   if (bluetooth.available())
  {
   voice = Serial.readString();

  }
  
    if (voice == "front")       //move forwards
    {
      digitalWrite(motorRightA, HIGH);
      digitalWrite(motorLeftA,  HIGH);
    }
    else if (voice == "back")       //move backwards
    {
      digitalWrite(motorRightB, HIGH);
      digitalWrite(motorLeftB, HIGH);
    }
    else if (voice == "stop")     //stop!
    {
      digitalWrite(motorRightA, LOW);
      digitalWrite(motorRightB, LOW);
      digitalWrite(motorLeftA, LOW);
      digitalWrite(motorLeftB, LOW);
    }
     else if (voice == "right")    //right
    {
      digitalWrite(motorRightA, HIGH);
      digitalWrite(motorRightB, LOW);
      digitalWrite(motorLeftA, LOW);
      digitalWrite(motorLeftB, LOW);
    }
         else if (voice == "left")    //right
    {
      digitalWrite(motorRightA, LOW);
      digitalWrite(motorRightB, LOW);
      digitalWrite(motorLeftA, HIGH);
      digitalWrite(motorLeftB, LOW);
    }
        else if (voice == "lights on")    //Lights on
    {
   digitalWrite(light, HIGH);
    }
        else if (voice == "lights off")    //Lights on
    {
   digitalWrite(light, LOW);
    }
       else if (voice == "alarm") {
    digitalWrite(buzzer, HIGH);
  }
      else if (voice == "reset") {
    digitalWrite(buzzer, LOW);
 }   

        else if (voice == "connect") {
    digitalWrite(buzzer, HIGH);
       digitalWrite(light, HIGH);
       delay(3000);
       digitalWrite(buzzer, LOW);
       digitalWrite(light, LOW);
 }    
 }
