# Hexapod
<!--Replace this text with a brief description (2-3 sentences) of your project. This description should draw the reader in and make them interested in what you've built. You can include what the biggest challenges, takeaways, and triumphs from completing the project were. As you complete your portfolio, remember your audience is less familiar than you are with all that your project entails!-->

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Zia S. | Archbishop Mitty Highschool | Computer Engineering | Rising Freshman |


![Picture of Zia](Zia-Headshot-Large.png)
  
# Final Milestone 
<!--For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE-->

<h1>Summary</h1>
<h2>Project</h2>
My project is the Hexapod, a robot that walks on 6 legs using multiple servos and can be controlled with a computer, remote, and phone. <br>

<h2>Components</h2>
<ul>
  <li>Ultrasonic Sensor</li>
  <li>0.96'' OLED Display</li>
  <li>Jumper Cables</li>
  <li>Small Breadboard</li>
</ul>

<h2>How Components Work Together</h2>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The ultrasonic sensor measures distance by sending out a 40,000 hertz sound wave and receives the pulse coming from the echo of the wave. The microcontroller (in this case the controller board) uses a program to calculate the distance by taking the time it took for the wave to come back using the equation distance is equal to time divided by 2 (taking into account wave going back and forth) times the speed of sound (340 m/s). The distance can be measured in any unit of measurement as long as the speed of sound is converted properly. The OLED display uses 8192 bits which turn on or off and using the Adafruit library, the OLED can be programmed to display text and images by turning off and on certain bits. The whole system works by using the ultrasonic sensor to get distance in cm and then the OLED takes the distance as an integer value and constantly displays the current distance. The same value is used to determine when the Hexapod should dance. The ultrasonic sensor and OLED display are both attached to a cardboard piece each to make it easier to mount to the Hexapod and the pins poke through to allow connection to the controller board from behind. 

<h2>What I Accomplished</h2> <!-- What I did in previous milestones and what I did for third milestone. -->
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; In the previous milestones, I assembled and calibrated the Hexapod and then programmed a dance and built a battery stand for it. For the third milestone, I added an ultrasonic sensor and OLED display to the Hexapod and programmed the Hexapod to display the distance that the ultrasonic sensor measures on the OLED. I also programmed it to walk around randomly and dance whenever the ultrasonic sensor detects a distance of 5 cm or less.  

<h2>Challenges & Triumps</h2>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Some challenges that I had during the project was assembling the Hexapod, building the battery stand, and wire managament. I had a lot of trouble attaching the legs to the Hexapod as they all had to be put in a certai orientation. It was very tough to attach them on the servo stands correctly due to some servos not fitting correctly and having to force them into position. It took 3 days, but I was able to fully assemble the Hexapod and do the final calibration step to fix any deviations in the legs. The most frustrating part of building the battery holder was measuring the holes correctly. It took me three tries to get the holes correct so that they would match with the Hexapod's standoffs and fit smoothly. Wire mangement was time-confusing as the breadboard, which held the ground and power wires, was at the bottom of the Hexapod on a cardboard platform. I had to set up the wires from the breadboard so that they were facing the right way for easier connection, but some wires were too long. As a result, I had to bunch up the wires inside the Hexapod so that no wries were pulled out by the legs which was very frustrating. The same was true for the wires of Trig and Echo pins (ultrasonic sensor) and the SDA and SCL pins (OLED display). It was hard to mount each device to the Hexapod as the wires resisted change. However, I triumphed over these challenges and produced a Hexapod that can measure distance and display it on an OLED display and dance. 

<h2>What I Learned</h2>
I learned many importatnt topics during the project, such as the basics of Arduino and using breadboards, coding with the Arduino IDE to control different devices such as an ultrasonic sensor, LEDs, and piezo buzzers, how servo motors work, check for circuit short, and using a drill and saw.

<h2>Next Step</h2>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; After learning everything I have learned at BlueStamp Engineering, I really want learn more advanced concepts in robotics and electrical engineering. <br><br> 

<h2>Hexapod_Ultrasonic_OLED.ino</h2>

```c++
#include <FNHR.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128 // OLED display width, in pixels
#define SCREEN_HEIGHT 64 // OLED display height, in pixels

// Declaration for an SSD1306 display connected to I2C (SDA, SCL pins)
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);

FNHR robot;
const int trigPin = 2;
const int echoPin = 3;
long duration;
int distanceCm;
int danceCm = 5;
bool Walk;

void setup() 
{
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  // OLED setup
  Serial.begin(115200);

  if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) { // Address 0x3D for 128x64
    Serial.println(F("SSD1306 allocation failed"));
    for(;;);
  }
  delay(2000);
  robot.Start();
  int distanceCm = 100;
  Walk = true; 
}
void loop()
{
  //Ultrasonic sensor measuring
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distanceCm = duration * 0.034 / 2;
  Serial.println(distanceCm);

  // Robot walking around randomly
  if (distanceCm > danceCm && Walk == true)
  {
    robot.CrawlForward();
  }
  
  if (distanceCm <= 5)
  { 
    Walk == false; 
    Dance();
  } 
  
  // Distance is less than 10
  if (distanceCm < 10)
  {
    display.clearDisplay();
    display.setTextSize(9);
    display.setTextColor(WHITE);
    display.setCursor(40, 0);
    display.print(distanceCm);
    display.display(); 
  }

  // Distance more or equal to 10 and less than 100
  if (distanceCm >= 10 && distanceCm < 100)
  {
    display.clearDisplay();
    display.setTextSize(9);
    display.setTextColor(WHITE);
    display.setCursor(15, 0);
    display.print(distanceCm);
    display.display(); 
  }

  // Distance more or equal to 100 and less or equal to than 400
  if (distanceCm >= 100 && distanceCm <= 400)
  {
    display.clearDisplay();
    display.setTextSize(7);
    display.setTextColor(WHITE);
    display.setCursor(1, 10);
    display.print(distanceCm);
    display.display(); 
  }

  // Distance more than 400
  if (distanceCm > 400)
  {
    display.clearDisplay();
    display.setTextSize(4);
    display.setTextColor(WHITE);
    display.setCursor(30, 0);
    display.println("Too");
    display.setCursor(30,30);
    display.println("Far");
    display.display(); 
  }
  
}

void Dance()
{
  robot.SetActionSpeed(50);

  for (int i = 0; i < 5; i++)
  {
    robot.CrawlBackward();
  }

  delay(500);

  //Max height is 50

  // Dance Move 1
  for (int i = 0; i < 4; i++)
  {
    robot.SetActionSpeed(200);
    robot.ChangeBodyHeight(50);
    robot.ChangeBodyHeight(30);
  }
  
  // Dance Move 2
  robot.RotateBody(100, 0, 0);
  robot.RotateBody(100, 100, 0);
  robot.RotateBody(0, 100, 0);
  robot.RotateBody(-100, 100, 0);
  robot.RotateBody(-100, 0, 0);
  robot.RotateBody(-100, -100, 0);
  robot.RotateBody(0, -100, 0);
  robot.RotateBody(100, -100, 0);
  robot.RotateBody(0, 0, 0);

  // Dance Move 3

  // Each turn command is 5.625 degrees ish
  for (int i = 0; i < 8; i++)
  {
    robot.TurnRight();
  }

  //Body twist
  robot.RotateBody(0, 0, 100);
  robot.RotateBody(0, 0, -100);
  robot.RotateBody(0, 0, 0);

  // Dance Move 4

  // Square dance
  for (int i = 0; i < 2; i++)
  {
    robot.CrawlForward();
    robot.CrawlRight();
    robot.CrawlBackward();
    robot.CrawlLeft();
    robot.CrawlForward();
  }

  // Dance Move 5

  for (int i = 0; i < 8; i++)
  {
    robot.TurnLeft();
  }

  robot.ChangeBodyHeight(0);
  robot.ChangeBodyHeight(50);

  for (int i = 0; i < 8; i++)
  {
    robot.TurnLeft();
  }

  // Dance Move 6

  //Body twist
  robot.RotateBody(0, 0, 100);
  robot.RotateBody(0, 0, -100);
  robot.RotateBody(0, 0, 0);

  // Square dance
  for (int i = 0; i < 2; i++)
  {
    robot.CrawlForward();
    robot.CrawlRight();
    robot.CrawlBackward();
    robot.CrawlLeft();
    robot.CrawlForward();
  }

  delay(200);

  // Dance Move 7

  for (int i = 0; i < 8; i++)
  {
    robot.TurnRight();
  }

  robot.CrawlRight();
  delay(500);
  robot.CrawlLeft();
  robot.CrawlLeft();
  delay(600);

  for (int i = 0; i < 4; i++)
  {
    robot.CrawlLeft();
    robot.CrawlRight();
  }

  delay(900);
  robot.CrawlRight();

  // Bowing
  delay(800);
  robot.ChangeBodyHeight(50);
  robot.RotateBody(50, 0, 0);
  robot.ChangeBodyHeight(50);
  robot.TurnRight();
  robot.RotateBody(50, 0, 0);
  robot.ChangeBodyHeight(50);
  robot.TurnLeft();
  robot.TurnLeft();
  robot.RotateBody(50, 0, 0);
  robot.RotateBody(0, 0, 0);
  robot.TurnRight();
  robot.ChangeBodyHeight(50);

  delay(100);

  for (int i = 0; i < 16; i++)
  {
    robot.TurnRight();
  }
  distanceCm = 100;
  Walk = true;
}

```

<!--**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**-->

<!--<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>-->

# Second Milestone

<h1>Summary</h1>
<h2>Project</h2>
My project is the Hexapod, a robot that walks on 6 legs using multiple servos and can be controlled with a computer, remote, and phone. <br>

<h2>Components</h2>
<ul>
  <li>Acrylic Parts</li>
  <li>Calibration Graph</li>
  <li>Servo Package</li>
  <li>Freenove Crawling Robot Controller</li>
  <li>WLAN Module</li>
  <li>USB Cable</li>
</ul>

<h2>How Components Work Together</h2>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The acrylic parts make up the body of the Hexapod and hold the servos and robot controller in place. The robot controller controls all of the servos in order to perform actions such as walking, turning, or raising body height. The WLAN module that is attached to the Hexapod creates a hotspot that can be connected to through a computer or phone and then the Hexapod can be controlled through an app or the Processing software on the computer. The USB cable allows for Arduino sketches to be uploaded to the robot controller so that the Hexapod can have a custom program. 

<h2>Progress</h2>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For the second milestone, I cut out and drilled a battery holder that goes on top of the Hexapod to hold the 7.2 volt battery. The Hexapod is now able to move around without being dragged down by the battery. I also programmed a dance for the Hexapod using the FNHR library to access functions that control certain aspects of the Hexapod's movement, such as walking and turning. Only a few hours were necessary to program the dance, and I then uploaded the sketch to the control board so that whenever I turn on the Hexapod, it does a dance using many functions such as a robot.CrawlForward and robot.Rotate().

<h2>Challenges Faced</h2>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;At first, I used Onshape, a 3D modeling software, to design a battery holder for the Hexapod. I later found out that due to the model's overhangs, it would take a very long time to 3D print the model and the necessary supports. With this in mind, I created a new 3D model for the battery holder, a plate with holes that fit screws, and the battery is held down to the plate using a hot-glued velcro strap. Although, one of my mentors, Ricky, said that it would be faster to cut out a wood plate by hand rather than print it since the 3D model was not intricate enough to justify using a 3D printer. The first time I cut out a battery holder and drilled holes for the screws, the holes did not match with Hexapod so I had to make a new plate. The second plate's also did not match and I later found out that I was using the wrong side of the caliper, a digital ruler, for measuring. For the third plate, I had to be very precise with the measurements, down to the micrometer. <br>

<h2>Next Step</h2>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;With the battery holder, the Hexapod can now go anywhere, contributing to my final milestone: adding an ultrasound sensor to the Hexapod and programming it to randomly walk around a room and use the ultrasound sensor to determine when it should dance. The second part of the final milestone is adding an LCD (Liquid Crystal Display) that shows what distance the ultrasound sensor measures. Before starting my final milestone, I need to learn more about ultrasound sensors and LCDs, how they work, and how to program them with Arduino. <br> <br>

<img src="Picture-Hexapod-V2.jpg"  width="700" height="523.4107">

<img src="Picture-Battery-Holder.jpg"  width="650" height="823.5069">

<iframe width="901" height="507" src="https://www.youtube.com/embed/BK9GC6PL-00" title="Zia S. Milestone 2" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<h2>Hexapod_Dance.ino</h2>

```c++
#include <FNHR.h>

FNHR robot;
int rotate = 20;

void Dance()
{
  robot.SetActionSpeed(50);

  for (int i = 0; i < 5; i++)
  {
    robot.CrawlBackward();
  }

  delay(500);

  //Max height is 50

  // Dance Move 1
  for (int i = 0; i < 4; i++)
  {
    robot.SetActionSpeed(200);
    robot.ChangeBodyHeight(50);
    robot.ChangeBodyHeight(30);
  }
  

  // Dance Move 2
  robot.RotateBody(100, 0, 0);
  robot.RotateBody(100, 100, 0);
  robot.RotateBody(0, 100, 0);
  robot.RotateBody(-100, 100, 0);
  robot.RotateBody(-100, 0, 0);
  robot.RotateBody(-100, -100, 0);
  robot.RotateBody(0, -100, 0);
  robot.RotateBody(100, -100, 0);
  robot.RotateBody(0, 0, 0);

  // Dance Move 3

  // Each turn command is 5.625 degrees ish
  for (int i = 0; i < 8; i++)
  {
    robot.TurnRight();
  }

  //Body twist
  robot.RotateBody(0, 0, 100);
  robot.RotateBody(0, 0, -100);
  robot.RotateBody(0, 0, 0);

  // Dance Move 4

  // Square dance
  for (int i = 0; i < 2; i++)
  {
    robot.CrawlForward();
    robot.CrawlRight();
    robot.CrawlBackward();
    robot.CrawlLeft();
    robot.CrawlForward();
  }

  // Dance Move 5

  for (int i = 0; i < 8; i++)
  {
    robot.TurnLeft();
  }

  robot.ChangeBodyHeight(0);
  robot.ChangeBodyHeight(50);

  for (int i = 0; i < 8; i++)
  {
    robot.TurnLeft();
  }

  // Dance Move 6

  //Body twist
  robot.RotateBody(0, 0, 100);
  robot.RotateBody(0, 0, -100);
  robot.RotateBody(0, 0, 0);

  // Square dance
  for (int i = 0; i < 2; i++)
  {
    robot.CrawlForward();
    robot.CrawlRight();
    robot.CrawlBackward();
    robot.CrawlLeft();
    robot.CrawlForward();
  }

  delay(200);

  // Dance Move 7

  for (int i = 0; i < 8; i++)
  {
    robot.TurnRight();
  }

  robot.CrawlRight();
  delay(500);
  robot.CrawlLeft();
  robot.CrawlLeft();
  delay(600);

  for (int i = 0; i < 4; i++)
  {
    robot.CrawlLeft();
    robot.CrawlRight();
  }

  delay(900);
  robot.CrawlRight();

  // Bowing
  delay(800);
  robot.ChangeBodyHeight(50);
  robot.RotateBody(50, 0, 0);
  robot.ChangeBodyHeight(50);
  robot.TurnRight();
  robot.RotateBody(50, 0, 0);
  robot.ChangeBodyHeight(50);
  robot.TurnLeft();
  robot.TurnLeft();
  robot.RotateBody(50, 0, 0);
  robot.RotateBody(0, 0, 0);
  robot.TurnRight();
  robot.ChangeBodyHeight(50);

  delay(100);


}

void setup() 
{
  robot.Start();
  Dance();
}

void loop() 
{
  
  
}
```

<!--<iframe width="560" height="315" src="https://www.youtube.com/embed/y3VAmNlER5Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>-->

# First Milestone

<h1>Summary</h1>

<h2>Project</h2>
My project is the Hexapod, a robot that walks on 6 legs using multiple servos and can be controlled with a computer, remote, and phone. <br>

<h2>Components</h2>
<ul>
  <li>Acrylic Parts</li>
  <li>Calibration Graph</li>
  <li>Servo Package</li>
  <li>Freenove Crawling Robot Controller</li>
  <li>WLAN Module</li>
  <li>USB Cable</li>
</ul> <br>

<h2>How Components Work Together</h2>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The acrylic parts make up the body of the Hexapod and hold the servos and robot controller in place. The robot controller controls all of the servos in order to perform actions such as walking, turning, or raising body height. The WLAN module that is attached to the Hexapod creates a hotspot that can be connected to through a computer or phone and then the Hexapod can be controlled through an app or the Processing software on the computer. The USB cable allows for Arduino sketches to be uploaded to the robot controller so that the Hexapod can have a custom program. 

<h2>Progress</h2>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;For the first milestone, I assembled the whole Hexapod and downloaded the necessary software, such as the Arduino libraries FlexiTimer.zip, FNHR.zip, and RF24.zip and the Processing IDE, to control the Hexapod using my computer, a remote, or my phone. I was also able to calibrate the legs of the Hexapod with the Processing IDE so that the Hexapod would function correctly. 

<h2>Challenges Faced</h2>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The first day of the building was a bust as I screwed the servo stands (what the servos will be attached to) on the wrong side of the main body of the Hexapod and unfortunately had to unscrew everything which took up the rest of the day. On the second day, I screwed the servo stands on the main body correctly and moved on to screwing servo stands to the various pieces of each leg. I then assembled the bases of each leg which included two servos, one to be attached to the main body and another to the leg. Then came the most frustrating part of assembly: attaching the servos. I first had to power the control board using a non-lithium battery and then connect each servo to the board so that the servos would rotate to their default position. I also had to attach each servo to each servo stand at a certain angle which took up a lot of time and went into the third day. On the third day, the control board's battery cable fritzed and I had to recalibrate the servos again because the new board had a different default setting for the servo's rotation. 

<h2>Next Step</h2>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The next step is completing my second milestone: adding a battery holder to the Hexapod and programming a dance.<br><br>


<img src="Picture-Processing-Software.png"  width="650" height="518.8596">

<img src="Picture-Hexapod.png"  width="650" height=" 616.6875">

<h2>Robot.ino</h2>
  
```c++
/*
* Sketch     Default function sketch for robot
* Platform   Freenove Hexapod Robot (Compatible with Arduino/Genuino Mega 2560)
* Brief      This sketch is used to show default function of Freenove Hexapod Robot.
*            You can control the robot by remote control, Android device or computer.
*            Changing the code will make the default function not working properly.
* Author     Ethan Pan @ Freenove (support@freenove.com)
* Date       2020/04/24
* Copyright  Copyright © Freenove (http://www.freenove.com)
* License    Creative Commons Attribution ShareAlike 3.0
*            (http://creativecommons.org/licenses/by-sa/3.0/legalcode)
* -----------------------------------------------------------------------------------------------*/

#ifndef ARDUINO_AVR_MEGA2560
#error Wrong board. Please choose "Arduino/Genuino Mega or Mega 2560"
#endif

// Include FNHR (Freenove Hexapod Robot) library
#include <FNHR.h>

FNHR robot;

void setup() {
  // Start Freenove Hexapod Robot with default function
  robot.Start(true);
}

void loop() {
  // Update Freenove Hexapod Robot
  robot.Update();
}

```

<iframe width="901" height="507" src="https://www.youtube.com/embed/74lIREHhu28" title="Zia S. Milestone 1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<!--# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 


# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
|:--:|:--:|:--:|:--:|
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
|:--:|:--:|:--:|:--:|
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
|:--:|:--:|:--:|:--:|-->

# Starter Project

My Starter Project is the Custom Arduino Project, in which I used a Piezo buzzer to play 4 different sounds by pressing a different button. There is also a fifth button that increases the hertz of each sound by 100 to play new sounds.

Materials:
* 1 Arduino Uno Microboard
* 1 Arduino Protoshield
* 1 Piezo Buzzer
* 4 10k Ohm Resistors
* 5 Pushbuttons
* 14 Male-To-Male Jumper Cables
* 1 Solder & Soldering Iron
* 1 USB Cable
* 1 Large Breadboard

Procedure: 
1. First, I built my circuit: I attached 4 buttons on the middle of the breadboard and set up each button by connecting each button to a pin (pin 1, 2, 3, & 4), adding a resistor, and adding another jumper cable to the extended side of the breadboard. I then added the Piezo Buzzer, connected a jumper cable to pin 8, and connected another jumper cable to the extended side. Finally, I connected a jumper cable to ground and another one to the 5 volt pin.
2. Next, I made an Arduino sketch that makes the piezo buzzer play a sound depending on the button pressed.
3. I connected the Arduino Uno to my computer using an USB cable and ran the code, able to play a sound for each button.
4. Afterwards, I added a fifth button that changed the sound that was played when pressed.

<h2>Piezo-Buzzer-DJ.ino</h2>

```c++
const int buttonOne = 1;
const int buttonTwo = 2;  
const int buttonThree = 3;
const int buttonFour = 4;
const int buttonFive = 5;
const int piezoBuzzer = 8;
int sound = 100;

void setup()
{
  // put your setup code here, to run once:
  pinMode(buttonOne, INPUT);
  pinMode(buttonTwo, INPUT);
  pinMode(buttonThree, INPUT);
  pinMode(buttonFour, INPUT);
  pinMode(buttonFive, INPUT);
  pinMode(piezoBuzzer, OUTPUT);
}

void loop() 
{
  // put your main code here, to run repeatedly:
  int buttonOneState, buttonTwoState, buttonThreeState, buttonFourState, buttonFiveState; 

  buttonOneState = digitalRead(buttonOne);
  buttonTwoState = digitalRead(buttonTwo);
  buttonThreeState = digitalRead(buttonThree);
  buttonFourState = digitalRead(buttonFour);
  buttonFiveState = digitalRead(buttonFive);

  // Pressing Button 1
  if (buttonOneState == LOW)
  {
    //digitalWrite(ledPin, HIGH);
    tone(piezoBuzzer, sound + 100 , 100);
  }
  else
  {
    
  }

  // Pressing Button 2
  if (buttonTwoState == LOW)
  {
    tone(piezoBuzzer, sound + 500, 100);
  }
  else
  {
    // turn off stuff
  }

  // Pressing Button 3
  if (buttonThreeState == LOW)
  {
    tone(piezoBuzzer, sound + 1400, 100);
  }
  else
  {
    // turn off stuff
  }

  // Pressing Button 4
  if (buttonFourState == LOW)
  {
    tone(piezoBuzzer, sound + 400, 100);
  }
  else
  {
    // turn off stuff
  }

  if (buttonFiveState == LOW)
  {
    sound += 100;
  }
  else
  {
    
  }

}
```

![Picture of Starter Project](Piezo-Buzzer-Picture.jpg)

<iframe width="901" height="507" src="https://www.youtube.com/embed/VHd2mac3Z-E" title="Zia S. Starter Project" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<!--# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.-->
