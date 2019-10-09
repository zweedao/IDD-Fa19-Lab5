# Jack in the Box

Lab report by Zwee Dao

For your write up, include:

## 1) Your Arduino code.

<pre><code>
#include <Servo.h>

// initialize the rotary encoder  
#define ENC_A 6 //these need to be digital input pins
#define ENC_B 7

Servo myservo;  // create servo object to control a servo
// twelve servo objects can be created on most boards

int pos = 0;    // variable to store the servo position

void setup() {
  myservo.attach(9);  // attaches the servo on pin 9 to the servo object
  myservo.write(pos);
  
  /* Setup encoder pins as inputs */
  pinMode(ENC_A, INPUT_PULLUP);
  pinMode(ENC_B, INPUT_PULLUP);
 
  Serial.begin (9600);
  Serial.println("Start");
}

void loop() {
  static unsigned int counter4x = 0;      //the SparkFun encoders jump by 4 states from detent to detent
  static unsigned int counter = 0;
  static unsigned int prevCounter = 0;
  int tmpdata;
  tmpdata = read_encoder();
  if( tmpdata) {
    counter4x += tmpdata;
    counter = counter4x/4;
    if (prevCounter != counter){
      // print rotary encoder value to Serial
      Serial.print("Counter value: ");
      Serial.println(counter);
      
      // rotate motor based on counter
      //pos = map(counter,0,16383,0,180);
      if (counter > 0) {
        pos = 120;
      }
      else {
        pos = 0;
      }
      myservo.write(pos);
    }
    prevCounter = counter;
  }
}

/* returns change in encoder state (-1,0,1) */
int read_encoder()
{
  static int enc_states[] = {
    0,-1,1,0,1,0,0,-1,-1,0,0,1,0,1,-1,0  };
  static byte ABab = 0;
  ABab *= 4;                   //shift the old values over 2 bits
  ABab = ABab%16;      //keeps only bits 0-3
  ABab += 2*digitalRead(ENC_A)+digitalRead(ENC_B); //adds enc_a and enc_b values to bits 1 and 0
  return ( enc_states[ABab]);
}

</code></pre>

## 2) .stl or .svg files for your Jack — if you use some other technique, include the respective supporting material.

[STL file](/zwee_in_box.stl)

## 3) At least one photo of your box taken in the MakerLab's Portable Photo Studio (or somewhere else, but of similar quality).

![Jack photo](/jack.JPG)

## 4) A video of your box in action.

[Jack video](/jack2.mov)
