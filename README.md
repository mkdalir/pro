

#include <Arduino.h>

#include <NewPing.h>


#define TRIGGER_PIN 9
#define ECHO_PIN 10
#define BUZZER A0
#define SWITCH A1

#define MAX_DISTANCE 400

NewPing sensor(TRIGGER_PIN, ECHO_PIN, MAX_DISTANCE);


//ARDUINO LINE FOLLOWING CAR//
// YOU HAVE TO INSTALL THE AFMOTOR LIBRARY BEFORE UPLOAD THE CODE//
// GO TO SKETCH >> INCLUDE LIBRARY >> ADD .ZIP LIBRARY >> SELECT AF MOTOR ZIP FILE //
 
//including the libraries
#include <AFMotor.h>
//defining pins and variables
#define left A4
#define right A5

int mot = 0;
//defining motors
AF_DCMotor motor1(1, MOTOR12_1KHZ); 
AF_DCMotor motor2(2, MOTOR12_1KHZ);
AF_DCMotor motor3(3, MOTOR34_1KHZ);
AF_DCMotor motor4(4, MOTOR34_1KHZ);




void setup() {
  //declaring pin types
  pinMode(left,INPUT);
  pinMode(right,INPUT);
  pinMode(BUZZER, OUTPUT);
  pinMode(SWITCH, INPUT_PULLUP);
  //begin serial communication
  Serial.begin(9600);

  // digitalWrite(BUZZER, HIGH);
  // analogWrite(BUZZER, HIGH);

  
} 
void loop() {

  Serial.println();

  if (analogRead(SWITCH) <= 10) {
    analogWrite(BUZZER, 200);
  } else {
    analogWrite(BUZZER, 0);
  }
}
void loop(){ 



  if (GPIOR02==0)
  {
    ultra.("");
  } else {
    sensor.("");
  }

  
  
  mot = sensor.ping_cm();
  Serial.print("DIstance = ");
  Serial.print(mot);
  Serial.println("cm");
  delay(800);


  if(mot <= 10) {
    Serial.println("OK");
    motor1.run(FORWARD);
    motor1.setSpeed(200);
    motor4.run(FORWARD);
    motor4.setSpeed(200);
  } else {
    motor1.run(RELEASE);
    motor1.setSpeed(0);
    motor4.run(RELEASE);
    motor4.setSpeed(0);
  }

  
  
  //printing values of the sensors to the serial monitor
  Serial.println(digitalRead(left));
  
  Serial.println(digitalRead(right));
  //line detected by both
  if(digitalRead(left)==0 && digitalRead(right)==0){
    //Forward
    motor1.run(FORWARD);
    motor1.setSpeed(200);
    motor4.run(FORWARD);
    motor4.setSpeed(200);
    analogWrite(2,HIGH);

  }
  //line detected by left sensor
  else if(digitalRead(left)==0 && !analogRead(right)==0){
    //turn left
    motor1.run(FORWARD);
    motor1.setSpeed(200);
    motor4.run(BACKWARD);
    motor4.setSpeed(200);
    analogWrite(2,HIGH);

    
  }
  //line detected by right sensor
  else if(!digitalRead(left)==0 && digitalRead(right)==0){
    //turn right
    motor1.run(BACKWARD);
    motor1.setSpeed(200);
    motor4.run(FORWARD);
    motor4.setSpeed(200);
    analogWrite(2,HIGH);
   
  }
  //line detected by none
  else if(!digitalRead(left)==0 && !digitalRead(right)==0){
    //stop
    motor1.run(RELEASE);
    motor1.setSpe0 ed(0);
    motor4.run(RELEASE);
    motor4.setSpeed(0);
    analogWrite(2,LOW),
    
    

    
   
  }

  
  
} 



void beep() {
  //  analogWrite(BUZZER, 200);
  // delay(1000);
  // analogWrite(BUZZER, 0);
  // delay(1000);
}

