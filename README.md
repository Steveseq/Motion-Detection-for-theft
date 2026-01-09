# Motion-Detection-for-theft
Interfacing-PIR-sensor-using-IR-sensor-for-theft-detection I created an IOT based theft detection system using Arduino Uno, IR sensor, Arduino IDE and PIR Motion Sensor. For monitoring changes in infrared radiation, When motion is detected, the PIR sensor sends out a signal in the serial monitor of Arduino IDE that motion is detected.

Code:
// Devloper and Owner: Steve Sequeira
//the time we give the sensor to calibrate (10-60 secs according to the datasheet that is serial
monitor)
int calibrationTime = 30;
//the time when the sensor outputs a low impulse
long unsigned int lowIn;
//the amount of milliseconds the sensor has to be low
//before we assume all motion has stopped
long unsigned int pause = 5000;
boolean lockLow = true;
boolean takeLowTime;
int pirPin = 3; //the digital pin connected to the PIR sensor's output
int ledPin = 13;
/////////////////////////////
//SETUP
void setup(){
Serial.begin(9600);
pinMode(pirPin, INPUT);
pinMode(ledPin, OUTPUT);
digitalWrite(pirPin, LOW);
//give the sensor some time to calibrate
Serial.print("calibrating sensor ");
for(int i = 0; i < calibrationTime; i++){
Serial.print(".");
delay(1000);
}
Serial.println(" done");
Serial.println("SENSOR ACTIVE");
delay(50);
}
////////////////////////////
//LOOP
void loop(){
if(digitalRead(pirPin) == HIGH){
digitalWrite(ledPin, HIGH); //the led visualizes the sensors output pin state
if(lockLow){
//makes sure we wait for a transition to LOW before any further output is made:
lockLow = false;
Serial.println("---");
Serial.print("motion detected at ");
Serial.print(millis()/1000);
Serial.println(" sec");
delay(50);
}
takeLowTime = true;
}
if(digitalRead(pirPin) == LOW){
digitalWrite(ledPin, LOW); //the led visualizes the sensors output pin state
if(takeLowTime){
lowIn = millis();
takeLowTime = false;
}
//save the time of the transition from high to LOW
//make sure this is only done at the start of a LOW phase
//if the sensor is low for more than the given pause,
//we assume that no more motion is going to happen
if(!lockLow && millis()- lowIn > pause){
//makes sure this block of code is only executed again after
//a new motion sequence has been detected
lockLow = true;
Serial.print("motion ended at ");
//output
Serial.print((millis()- pause)/1000);
Serial.println(" sec");
delay(50);
}
}
}
// Devloper and Owner: Steve Sequeira
int IRSensor = 9; // connect ir sensor module to Arduino pin 9
int LED = 13; // connect LED to Arduino pin 13
void setup()
{
}
Serial.begin(115200); // Init Serial at 115200 Baud
Serial.println("Serial Working"); // Test to check if serial is working or not
pinMode(IRSensor, INPUT); // IR Sensor pin INPUT
pinMode(LED, OUTPUT); // LED Pin Output
void loop()
{
int sensorStatus = digitalRead(IRSensor); // Set the GPIO as Input
if (sensorStatus == 1) // Check if the pin high or not
{
// if the pin is high turn off the onboard Led
digitalWrite(LED, LOW); // LED LOW
Serial.println("Motion Ended!"); // print Motion Detected! on the serial monitor window
}
else
{
//else turn on the onboard LED
digitalWrite(LED, HIGH); // LED High
Serial.println("Motion Detected!"); // print Motion Ended! on the serial monitor window
}
