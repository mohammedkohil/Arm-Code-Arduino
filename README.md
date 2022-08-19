# Arm-Code-Arduino
Servo servo1;
Servo servo2;
Servo servo3;
Servo servo4;

int bot1 = A1;
int bot2 = A2;
int bot3 = A3;
int bot4 = A4;

int valPot1 = 120;
int valPot2 = 40;
int valPot3 = 60;
int valPot4 = 0;

bool isFirstIteration = true;

void setup() {
  Serial.begin(9600); 
  servo1.attach(3);
  servo2.attach(5);
  servo3.attach(6);
  servo4.attach(9); 

  servo1.write(valPot1);
  servo2.write(valPot2);
  servo3.write(valPot3);
  servo4.write(valPot4);
}

void setServoValue(String potName, int& pot, int& potVal, Servo servo, int min, int max, bool isMoveSlowly) {
  int analogVal = analogRead(pot);  
  analogVal = map(analogVal, 0, 1023, 0, 180);
  //Serial.print(" ");
  //Serial.print(analogVal);

  if(analogVal < min)
    analogVal = min;

  if(analogVal > max)
    analogVal = max;

  if(isMoveSlowly) {
    if(analogVal > potVal) 
      potVal++;
    else if(analogVal < potVal)
      potVal--;

    servo.write(potVal);
    delay (10);    
  }
  else {
    servo.write(analogVal);
    delay (70);
  }

  Serial.print(" " + potName + " ");
  Serial.print(potVal);
}


void loop() {

  if(isFirstIteration) {
    delay(2000);
    isFirstIteration = false;
  }

  setServoValue("bot1", bot1, valPot1, servo1, 85, 130, true);
  setServoValue("bot2", bot2, valPot2, servo2, 00, 60, true);
  setServoValue("bot3", bot3, valPot3, servo3, 0, 180, true);
  setServoValue("bot4", bot4, valPot4, servo4, 0, 120, false);
  Serial.println();
  
}
