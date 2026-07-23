//Robot specifications:
// 4 IR sensors
// 2 QTR sensors
// 2 750 rpm DC motors
// 1 starter
// 2s 800 mah lipo battery
// Notes:
// - Both motors only initializes with generic functions as .Forward . Backward .Right .Left .ArcTurn .MotorControl
// - For motor programming: two modes to use - With timecount (.Forward .Backward .Right .Left .ArcTurn) / Timeless (.MotorControl - signs for motor direction (+/-)) -
// - 
// Strategy (option #1): slow trayectory until sensors detect opponent, increase motors speed to launch an offensive.
// Strategy (option #2): medium speed at trayectory until sensors detect opponent, decrease motors speed to launch an offensive (improve inertia and wheels friction).
// Strategy (option #3): medium speed until sensors detect opponent, increase motors speed to launch an arcturn offensive so the opponent falls off the dohyo.

#include <xmotionV3.h> //xmotion library

int calib_left = 170;
int calib_right = 255;

int outer_left_IR = 0;
int front_left_IR = 1;
int front_right_IR = 2;
int outer_right_IR = 4;
int starter = A0;
int left_QTR = A5;
int right_QTR = A4;

int no_opp_cycles = 0; // count of cycles in which no opp was detected
const int search_count = 400; // time until robot starts search mode
const int search_turn  = 100;

//Motor calibration
void motor_calibration (int speed) {
  // Velocity scale depending the required %
  int left = map(speed, 0, 100, 0, calib_left);
  int right = map(speed, 0, 100, 0, calib_right);
  xmotion.MotorControl(left, right);
}

void setup() {
  Serial.begin(9600); //Establishes the speed of the transmission of serial data in bits per second 
  xmotion.StopMotors(100); //Stopping motors for resetting
  //Declare inputs and outputs
  pinMode(outer_left_IR,INPUT);
  pinMode(front_left_IR,INPUT);
  pinMode(front_right_IR,INPUT);
  pinMode(outer_right_IR,INPUT);
  pinMode(left_QTR,INPUT);
  pinMode(right_QTR,INPUT);
  pinMode(starter,INPUT);
}

void loop() {
  while (digitalRead(starter) == 0) {xmotion.UserLed2(200);}
  while(1){

    //xmotion.ArcTurn(11,42,5); // Moves forward slowly to approach opponent ---- then attack

    //Line sensors --- Stay inside the dohyo    
    if (analogRead(left_QTR) < 70 && analogRead(right_QTR) > 326 ){  //left line sensor detects line
      motor_calibration(-35); 
      delay(500); 
      xmotion.Left0(8, 15);
      //continue;
    } 
    else if (analogRead(left_QTR) > 350 && analogRead(right_QTR) < 60){ // right line sensor detects line
      motor_calibration(-35); 
      delay(500); 
      xmotion.Right0(9, 15);
      //continue;
    }
    else if (analogRead(left_QTR) < 70 && analogRead(right_QTR) < 60){ // both line sensors detect line
      motor_calibration(-40); 
      delay(500);
      xmotion.Left0(9, 15);
      //continue;
    }
    else {
    // Opponent sensors --- Attack

    bool front_left = digitalRead(front_left_IR)  == 1;
    bool front_right = digitalRead(front_right_IR) == 1;
    bool outer_left = digitalRead(outer_left_IR)  == 1;
    bool outer_right = digitalRead(outer_right_IR) == 1;

    if (front_left && front_right) //Both front sensors detect opponent
    {xmotion.ArcTurn(18,69,2);}

    else if (front_left && !front_right) // Front left detects opponent
    {xmotion.ArcTurn(10,4,2);} 

    else if(front_right && !front_left) //Front right detects ooponent
    {xmotion.ArcTurn(30 ,30,2);}

    else if(front_left && outer_left) // Both left sensors detect opponent
    {xmotion.ArcTurn(9,42,2);}

    else if(front_right && outer_right) // Both right sensors detec opponent
    {xmotion.ArcTurn(16,24,2);}

    else if(outer_left && !outer_right) // Outer left detects opponent
    {xmotion.Left0(60,5);}

    else if(outer_right && !outer_left) // Outer right detects opponent
    {xmotion.Right0(45,5);}

    // No opponent detected --- Search mode

    else if (!front_left && !front_right && !outer_left && !outer_right)
    {
      no_opp_cycles++;

      if (no_opp_cycles < search_count)
      {xmotion.StopMotors(20);}
        else if (no_opp_cycles < search_count + search_turn){
          xmotion.Right0(20,5);
        }
        else{
          no_opp_cycles = 0;
        }
    }
    }
  }
}
