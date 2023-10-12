int LED13 = 13;
int LED12 = 12;
int LED11 = 11;
int LED10 = 10;
int LED9 = 9;
int LED8 = 8;
int LED7 = 7;
int LED6 = 6;
int LED5 = 5;
int LED4 = 4;
int push_button3 = 3;
int push_button2 = 2;
int push_button1 = A0;
int flag1 = 0;
int flag2 = 0;
int flag3 = 0;
int currentFloor = 0; 
unsigned long past = 0;
const unsigned long floorChangeDelay = 1000;

void setup() {
  Serial.begin(9600);
  pinMode(push_button1, INPUT);
  pinMode(push_button2, INPUT);
  pinMode(push_button3, INPUT);
  pinMode(LED13, OUTPUT);
  pinMode(LED12, OUTPUT);
  pinMode(LED11, OUTPUT);
  pinMode(LED10, OUTPUT);
  pinMode(LED9, OUTPUT);
  pinMode(LED8, OUTPUT);
  pinMode(LED7, OUTPUT);
  pinMode(LED6, OUTPUT);
  pinMode(LED5, OUTPUT);
  pinMode(LED4, OUTPUT);
  digitalWrite(LED7, HIGH);
  digitalWrite(LED4, HIGH);
}

void loop() {
  unsigned long now = millis();
  
  int inputValue1 = digitalRead(push_button1);
  int inputValue2 = digitalRead(push_button2);
  int inputValue3 = digitalRead(push_button3);
//   if (now - past >= 500){
//     past = now;
//     flag2 = 1;
//   }
//   if(flag2 == 1){
//     digitalWrite(13,!(digitalRead(13)));
//     flag2 = 0;
//    }
// }
  if (inputValue1 == HIGH) {
    if (inputValue3 == HIGH) {
      moveToFloor(3);
      flag3 = 1;
    } else {
      moveToFloor(1);
      flag1 = 1;
    }
  } else if (inputValue2 == HIGH) {
    moveToFloor(2);
    flag2 = 2;
  } else if (inputValue3 == HIGH) {
    moveToFloor(3);
    flag3 = 1;
  }
}

void moveToFloor(int targetFloor) {
  if (currentFloor == targetFloor) {
    return; 
  }

  if (currentFloor < targetFloor) {
    for (int floor = currentFloor; floor < targetFloor; floor++) {
      moveElevator(floor + 1);
      
    }
  } else {
    for (int floor = currentFloor; floor > targetFloor; floor--) {
      moveElevator(floor - 1);
     
    }
  }

  currentFloor = targetFloor; // 현재 층을 업데이트
  updateFloorLEDs(currentFloor);
}

void moveElevator(int floor) {
  
  switch (floor) {
    case 1:
      digitalWrite(LED7, HIGH);
      digitalWrite(LED4, HIGH);
      digitalWrite(LED5, LOW);
      digitalWrite(LED10, LOW);
      digitalWrite(LED6, LOW);
      digitalWrite(LED13, LOW);
      Serial.println("1F");
      break;
    case 2:
      digitalWrite(LED5, HIGH);
      digitalWrite(LED10, HIGH);
      digitalWrite(LED7, LOW);
      digitalWrite(LED4, LOW);
      digitalWrite(LED5, LOW);
      digitalWrite(LED6, LOW);
      digitalWrite(LED13, LOW);
      Serial.println("2F");
      break;
    case 3:
      digitalWrite(LED6, HIGH);
      digitalWrite(LED13, HIGH);
      digitalWrite(LED5, LOW);
      digitalWrite(LED10, LOW);
      Serial.println("3F");
      break;
  }
}

void updateFloorLEDs(int floor) {
 
  switch (floor) {
    case 1:
      digitalWrite(LED7, HIGH);
      digitalWrite(LED4, HIGH);
      digitalWrite(LED5, LOW);
      digitalWrite(LED6, LOW);
      Serial.println("led 1F");
      break;
    case 2:
      digitalWrite(LED5, HIGH);
      digitalWrite(LED7, LOW);
      digitalWrite(LED4, LOW);
      digitalWrite(LED6, LOW);
      Serial.println("led 2F");
      break;
    case 3:
      digitalWrite(LED6, HIGH);
      digitalWrite(LED5, LOW);
      Serial.println("led 3F");
      break;
    case 4:
  
      digitalWrite(LED6, HIGH);
      digitalWrite(LED5, LOW);
      Serial.println("led 3F");
      break;
    
  }
}
