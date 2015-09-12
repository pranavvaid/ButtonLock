//The following descriptions for variables show what they are used for in the code

const int button1 = 4;             //Define what pin button1 is at
const int button2 = 3;             //Define what pin button2 is at
const int redLed = 6;              //Define what pin the Red LED is at
const int greenLed = 5;            //Define what pin the Green LED is at
const int timeConstant = 2500;     //Change to higher number to allow more time between button presses
int lastEntry = 0;                 //The time the user inserted the previous digit of the code
boolean prevResult;                //Check if the user inserted the previous pin correctly
boolean sameButton;                //Check if the current digit and the next digit of the code is the same
boolean previResult;               //Check if the user inserted the previous pin correctly

/* The difference between prevResult and previResult: prevResult is automatically made true at the beginning of each loop so the user will be able to enter the first digit (otherwise if the user enters the wrong digit
the program will always see prevResult as false, and thus never check to see if a button is being pressed). The only way previResult can be made true is through a correct button being pressed. If I used prevResult in 
the lastButtonPressed function then just pressing the last button would result in the program believing that the whole code was entered. This is because in combinationFind, every function inside it is run, whether or not 
it was run successfully. 
*/

boolean fail;              //Check if the user failed to enter the correct digit
int greenLedState = 0;     //The current state of the green LED (0 = Dark, 1 = Bright)
int redLedState = 1;       //The current state of the red LED (0 = Dark, 1 = Bright)
int buttonPress = 0;       //The number of times a button has been pressed


void setup() {
  pinMode(button1,INPUT);
  pinMode(button2,INPUT);
  pinMode(redLed,OUTPUT);
  pinMode(greenLed,OUTPUT);
  digitalWrite(redLed, HIGH);
  attachInterrupt(3, checkCheating, FALLING);     //Run the function checkCheating whenever the button2 goes from HIGH to LOW.
  attachInterrupt(4, checkCheating, FALLING);     //Run the function checkCheating whenever the button1 goes from HIGH to LOW.
}


void loop() {
  lastEntry = millis();
  buttonPress = 0;
  combinationFind(button1,button2,button1,button2,button2,button1); //Change the sequence of buttons to change the code (Default Code: 1,2,1,2,2,1)
}//loop




void combinationFind (int p1,int p2,int p3,int p4,int p5,int p6){
  prevResult = true;
  buttonPressed(p1,p2);
  buttonPressed(p2,p3);
  buttonPressed(p3,p4);
  buttonPressed(p4,p5);
  buttonPressed(p5,p6);
  lastButtonPressed(p6);
  delay(10);
}//void
void buttonPressed (int b1,int b2){
  if (b1 == b2){             //Find out if the current button and the next button are the same
    sameButton = true;       //Write sameButton as true (the current digit and next digit are the same)
  } //if
  else{
    sameButton = false;      //Write sameButton as false (the current digit and next digit are not the same)
  }//else
  if (prevResult == true){
    while(millis() - lastEntry < timeConstant && prevResult == true){      //while 2.5 seconds haven't passed and the previous digit entered was correct
      if(sameButton == true){                                              //If the two buttons are the same run this code
        if(digitalRead(b1) == HIGH){                                       //If the correct button is pressed
          while(digitalRead(b1) != LOW){                                   //Only move on to the next line when the current button is let go of, so that holding down the button won't register for the next digit of the code
            }//while
          prevResult = false;
          previResult = false;
          delay(10);                                                       //Delay 10 milliseconds so the switch can be debounced
          lastEntry = millis(); 
          fail = false;
          break;                                                           //Exit the while loop
        }//if
      }//if
      else{
        if(digitalRead(b1) == HIGH && digitalRead(b2) == LOW){ //Check if the current button is being pressed and the other isn't, so that users can't push down on both buttons to pass
          while(digitalRead(b1) != LOW){
          }//while
          prevResult = false;
          previResult = false;
          delay(10);                                                      //Delay 10 milliseconds to debounce
          lastEntry = millis();
          lastEntry = millis();
          fail = false;
          break;                                                          //Exit the while loop
        }//if
        else if (digitalRead(b1) == LOW && digitalRead(b2) == HIGH){     //If the user presses the wrong button
          prevResult = false;
          previResult = false;
          fail = true;
          delay(100);
          lastEntry = millis() - 3000;
          break;                                //Exit the while loop
        }//else if
      }//else
    }//while
    prevResult = false;
    previResult = false;
  }//if
  if(fail == false){                 //If the user didn't fail to put in the right digit for the code
    prevResult = true;               // Rewrite the values prevResult and previResult so the program knows the user inserted the right digit
    previResult = true;
  }
  else{                             //If the user did fail to put in the right digit for the code
    prevResult = false;             // Rewrite the values prevResult and previResult so the program knows the user inserted the wrong digit
    previResult = false;
  }
}//void

void lastButtonPressed(int b1){
  if(previResult == true){
    while(millis()-lastEntry < timeConstant){
      if(digitalRead(b1)== HIGH){
        delay(10);
        lastEntry = millis();
        greenLedState = !greenLedState;           //Invert the state of the Green LED
        redLedState = !redLedState;               //Invert the state of the Red LED
        digitalWrite(greenLed,greenLedState);     //Rewrite the values for the LED
        digitalWrite(redLed,redLedState);         //Rewrite the values for the LED
        fail = false;
        prevResult = true;
        previResult = true;
        lastEntry = millis();
        break;
      }
    }
  }
}

void checkCheating(){
  buttonPress++;           //Increment button press every time a button is let go of
  delay(10);
  if(buttonPress>5){       //If a button is pressed more than 5 times then restart the code
  
                            /*The reason the value 5 was selected instead of 6 is because the 6th button press is the last button pressed. 
                            In the function lastButtonPressed the code doesn't check how many times a button was pressed after the final button is pressed.
                            This will only make a difference if the user presses buttons 6 times before they press the final button in the code
                            */
  
    prevResult = false;     //Change parameters so the program will know cheating has occured and will restart the code
    previResult = false;
    fail = true;
    delay(100);
    lastEntry = millis() - 3000;
    buttonPress = 0;        //reset the value of buttonPress
  }
}
