

### Lessons Learned
* Always make sure the sensor is the right input (digital vs analog)
* map() to values that make sense for your output (LED brightness, frequency, etc.)

### Outstanding questions
* Why does a phototransistor work with only digital input and ground? (Need to attach picture) 
 
### Ideas to Explore
* Connecting Arduino to TouchDesigner
* Creating a slider with 3 force sensors
* How to determine position (x,y) of an object on a conductive material using analog voltage readings

### Lab Work

[Digital Lab](https://itp.nyu.edu/physcomp/labs/labs-arduino-digital-and-analog/digital-input-and-output-with-an-arduino/)
[Analog Lab](https://itp.nyu.edu/physcomp/labs/labs-arduino-digital-and-analog/analog-in-with-an-arduino/)
[Basic Signal Processing](https://itp.nyu.edu/physcomp/labs/labs-arduino-digital-and-analog/lab-sensor-change-detection/ )


First was getting the digital input working with a simple button:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/eb2d7587-d0a7-4c32-beb8-8fe535b21353?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>


I had to swap out a few wires for testing and later tried two swap between two LEDS but failed:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/a8b30d68-4ad9-4579-9ed0-08bf7a730451?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>

```
// the setup function runs once when you press reset or power the board

void setup() {

  // initialize digital pin LED_BUILTIN as an output.

  pinMode(2, OUTPUT);

  pinMode(3, OUTPUT);

  pinMode(12, INPUT);

  

}

  
  

// the loop function runs over and over again forever

void loop() {

  if (digitalRead(12) == HIGH){

    Serial.println("CHECK");

    digitalWrite(2,HIGH);

    digitalWrite(3,LOW);

  }

  else{

    digitalWrite(2,LOW);

    digitalWrite(3,HIGH);

  }

}
```

I tested everything: my code, voltages, components but nothing seemed to be off. Thats when I realized I misplugged the wire meant for the 3.3V out into the 1st pin instead of the second. After that silly mistake it was now working:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/01aa5c54-5589-4e9b-9a01-d18718d0cde9?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>


After that was the first analog input, the poentiometer... which also failed:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/fa13ff9e-c988-48e2-a22b-870ad4b06896?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>


```

const int LED1 = 2;

const int POTENTIOMETER = 12;

int analogValue = 0;

int brightness = 0;

  

void setup() {

Serial.begin(9600);

// declare the led pin as an output:

pinMode(LED1, OUTPUT);

//pinMode(POTENTIOMETER, INPUT);

}

  

void loop() {

// put your main code here, to run repeatedly:

analogValue = analogRead(POTENTIOMETER);

//Serial.println(analogValue);

brightness = analogValue/4;

analogWrite(LED1, brightness);

Serial.println(brightness);

}
```


This was an easier fix, I realized quickly that the output of the potentiometer was going to a digital input. After that it was working, a very good reminder to check inputs and outputs BEFORE powering on:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/4798f83d-625b-4172-bb97-4b271b4f1b2c?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>


After that I could use the same code for any analog sensor, including the photoresistor and force sensor:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/7b818276-f9ef-4e46-881c-97709a4860b8?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>


<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/aad75dc7-1dec-4bc2-a5d9-e39590d8016c?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>


But I noticed something strange with the force sensor. It kept flickering even though I didn't touch it. At first I thought maybe, somehow, it was triggering without me touching it. 


<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/6f50a120-e2a3-45dc-8e44-5b880feecd8f?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>

I later realized that the sensor is actually quite sensitive and the threshold for producing light in the LED was very low, any movement around the sensor or the breeze from outside would be enough to trigger it. After re-ranging the output of the photoresistor, I was able to reduce how much light was passively being emitted:


<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/d53c0ef2-bb2d-408a-a8af-c1397c1961f1?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>


```
const int LED1 = 2;

const int PRESS1 = 6;

int analogValue1 = 0;

int brightness = 0;

  

void setup() {

    Serial.begin(9600);

    // declare the led pin as an output:

    pinMode(LED1, OUTPUT);

}

  

void loop() {

  // put your main code here, to run repeatedly:

  analogValue1 = analogRead(PRESS1);

  brightness = map(analogValue1,0,450,0,220);

  analogValue2 = analogRead(PRESS2);

  analogWrite(LED1, brightness);

  

  Serial.println(analogValue1);

  Serial.println(brightness);

  //Min 0 Max 450

  //

}
```


After that was tying two force reading sensors

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/3e6178d4-ba75-4f50-87c1-f6252135d376?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>

```  
const int LED1 = 2;

const int LED2 = 3;

const int PRESS1 = 6;

const int PRESS2 = 7;

int analogValue1 = 0;

int analogValue2 = 0;

  

void setup() {

Serial.begin(9600);

// declare the led pin as an output:

pinMode(LED1, OUTPUT);

pinMode(LED2, OUTPUT);

}

  

void loop() {

// put your main code here, to run repeatedly:

analogValue1 = analogRead(PRESS1);

analogValue2 = analogRead(PRESS2);

analogWrite(LED1, analogValue1);

analogWrite(LED2, analogValue2);

  

Serial.println("PRESS1 " + analogValue1);

Serial.println("PRESS2 " + analogValue2);

}
```


And using map to ease into the brightness, compare the two (Top with mapping to 0-220, Bottom with no mapping):
```  

const int LED1 = 2;

const int LED2 = 3;

const int PRESS1 = 6;

const int PRESS2 = 7;

int analogValue1 = 0;

int analogValue2 = 0;

int brightness = 0;

  

void setup() {

Serial.begin(9600);

// declare the led pin as an output:

pinMode(LED1, OUTPUT);

pinMode(LED2, OUTPUT);

}

  

void loop() {

// put your main code here, to run repeatedly:

analogValue1 = analogRead(PRESS1);

brightness = map(analogValue1,0,450,0,220);

analogValue2 = analogRead(PRESS2);

analogWrite(LED1, brightness);

analogWrite(LED2, analogValue2);

  

Serial.println(analogValue1);

Serial.println(brightness);

//Min 0 Max 450

//

}
```

