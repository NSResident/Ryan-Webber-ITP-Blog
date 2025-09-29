
### Lessons Learned
* Sound is much more complicated than I thought
* Transistors open up a whole new world of using a single controller and board to swap between different power supplies
### Outstanding questions
* Why do we need a capacitor for the servo motor?
* What is the reason the transistor makes the speaker louder in this example? I think it's because the speaker can get a full 3.3V from board as opposed to < 3.3.V when powered by the Arduino's output but not sure
	* **This is because the amperage from a data pin (~20mA) is significantly less than the vOut pin (~500mA), so the wattage  (V * A) is much lower. The speaker depends on wattage so the more wattage, the louder it is.**
![](https://PCompPull.b-cdn.net/Lab%203/Pasted%20image%2020250922201459.png)
 
### Ideas to Explore
* Mini DJing with two potentiometers and two pre-made songs
* How to determine position (x,y) of an object on a conductive material using analog voltage readings (More reading on Resistive and Capacitive sensors)

### Lab Work
[Tone](https://itp.nyu.edu/physcomp/labs/labs-arduino-digital-and-analog/tone-output-using-an-arduino/)
[Servo Motor](https://itp.nyu.edu/physcomp/labs/labs-arduino-digital-and-analog/servo-motor-control-with-an-arduino/)

The labs started off pretty simple as it mostly an extension of the last week, here I used a speaker and LED that gets modified by a potentiometer:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/75f319c6-3d12-4cef-aa8c-15f8cfb008c4?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>

```
int led1 = 2;

int speaker = 3;

int analogValue;

  

void setup() {

// put your setup code here, to run once:

// pinMode(led1, OUTPUT)

Serial.begin(9060);

pinMode(led1, OUTPUT);

}

  

void loop() {

// put your main code here, to run repeatedly:

analogValue = analogRead(A0);

  

analogWrite(led1, analogValue/4);

Serial.println(analogValue);

  

tone(speaker, 440, 1000);

delay(1000);

  
  

}
```


Changing the speaker frequency with potentiometer

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/865687b9-85ca-410c-9f70-768a3cc49fab?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>


```
int led1 = 2;

int speaker = 3;

int analogValue;

  

void setup() {

// put your setup code here, to run once:

// pinMode(led1, OUTPUT)

Serial.begin(9060);

pinMode(led1, OUTPUT);

}

  

void loop() {

// put your main code here, to run repeatedly:

analogValue = analogRead(A0);

  

analogWrite(led1, analogValue/4);

Serial.println(analogValue);

  

// tone(speaker, 440, 2000);

// delay(1000);

float frequency = map(analogValue, 0, 1023, 100, 4400);

tone(speaker, frequency, 500);

Serial.println("Frequency");

Serial.println(frequency);

  

}
```


Before doing the next lab, I watched the video on transistors. From my understanding, it's a switch that uses voltage as a signal to complete a circuit.

![](https://PCompPull.b-cdn.net/Lab%203/Pasted%20image%2020250916163448.png)


Current goes through the base a switch, which passes the voltage from the collector to the emitter. Admittedly it took me a long time to understand why using a transistor would increase the volume of the speaker. Reading the labs and watching videos didn't help me at all, and it wasn't until I really dug into the details with James that it started to make sense. 

Sound, as we hear it, is really a series of up and downs of a wave at certain frequencies. This analog signal isn't something naturally produced by an arduino, but using digital manipulation of releasing and holding the voltage, this analog signal behavior is simulated. When we connect the speaker directly to a digital output pin (figure 1 below), we get a simulated analog signal.  The speaker transforms this signal into sound, using the wave's frequency for the note and the amplitude for the volume.

![](https://PCompPull.b-cdn.net/Lab%203/IMG_4890.jpeg)


If we want to the amplitude to be higher (figure 2 above), we can use the power from the board (a consistent 3.3V) and instead of using the arduino's simulated analog signal to control the amplitude of the sound, we can use it to control when this 3.3V is on and off. It's not the transistor that makes the speaker louder, **its the higher voltage/current of directly connecting to the board.** The transistor allows the arduino to modulate the current, producing a wave that the speaker can transform into a sound.

![](https://PCompPull.b-cdn.net/Lab%203/IMG_4891.jpeg)


I probbaly didn't need to understand this much to continue doing the lab, and maybe that took away from the time I could have spent working on things, but assuming this is right I feel much more comfortable with circuits and in particular sound.

After this I continued with the lab and got a speaker with transistor playing 1 tone:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/1e876b65-09d4-4078-b790-7b55b50913cf?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>

```
void setup() {

// put your setup code here, to run once:

Serial.begin(9600);

  

}

  

void loop() {

// put your main code here, to run repeatedly:

tone(2, 440, 1000);

delay(1000);

}
```



Speaker with transistor playing 2 tones:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/9bb2a047-c8c5-41e2-9b76-841028834b58?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>

```
int speaker = 2;

  

void setup() {

// put your setup code here, to run once:

// pinMode(led1, OUTPUT)

Serial.begin(9060);

pinMode(speaker, OUTPUT);

}

  

void loop(){

  

tone(speaker, 440, 500);

delay(2000);

tone(speaker, 880, 2000);

delay(2000);

}
```


Using ServoMotors also wasn't too difficult, though I got it working without a capacitor so I need to understand a bit more on why it's necessary:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/81618b89-b3cb-4e15-8abb-6e84ccd49e12?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>

```
#include "Servo.h"

  

uint analogValue;

uint servoPin = 2;

long lastMoveTime = 0;

  

Servo servoMotor;

  

void setup() {

// put your setup code here, to run once:

Serial.begin(9600);

servoMotor.attach(servoPin);

}

  

void loop() {

// put your main code here, to run repeatedly:

analogValue = analogRead(A0);

Serial.println(analogValue);

int servAngle = map(analogValue,0,1023,0,179);

  

if (millis() - lastMoveTime > 20){

servoMotor.write(servAngle);

lastMoveTime = millis();

}

  

}
```

At the end I decided to combine both speaker and motor, which gave me some interesting ideas for a walking speaker:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/34aee875-e2d5-4dc1-9704-4282e26c75b3?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>

```
#include "Servo.h"

  

uint analogValue;

uint servoPin = 2;

long lastMoveTime = 0;

  

Servo servoMotor;

  

void setup() {

// put your setup code here, to run once:

Serial.begin(9600);

servoMotor.attach(servoPin);

}

  

void loop() {

// put your main code here, to run repeatedly:

analogValue = analogRead(A0);

Serial.println(analogValue);

int servAngle = map(analogValue,0,1023,0,179);

  

if (millis() - lastMoveTime > 20){

servoMotor.write(servAngle);

lastMoveTime = millis();

}

tone(3, 440, 1000);

delay(1030);

tone(3, 880, 1000);

delay(1030);

}
```