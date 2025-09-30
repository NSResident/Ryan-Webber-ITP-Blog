

##### I was very light on the lab this week and spent most of my time researching and starting initial circuit designs for the midterm project and brainstorming for the final. 

### Questions
- Typo in the lab [here](https://itp.nyu.edu/physcomp/labs/labs-arduino-digital-and-analog/lab-sensor-change-detection/#:~:text=You%20can%20see%20from%20this%20that%20you%E2%80%99ve%20actually%20got%20three%20states%20now%2C%20long%20press%20(%3E%20750ms)%2C%20short%20press%20(250%2D750ms)%2C%20and%20tap%20(%3E250ms).%20With%20this%2C%20you%20can%20make%20one%20button%20do%20three%20things.), should be tap (<250ms) not (>250ms)


### Lab Work

Simple state maintenance and count:

![](https://PCompPull.b-cdn.net/Lab%204/IMG_4993.jpeg)

```
uint8_t count = 0;

uint8_t lastButton = 0;

void setup() {

// put your setup code here, to run once:

Serial.begin(9600);

pinMode(2, INPUT);

  

}

  

void loop() {

// put your main code here, to run repeatedly:

long fsr_signal = analogRead(A0);

// Serial.println(fsr_signal);

  

int button = digitalRead(2);

  

if(button != lastButton && button == HIGH){

Serial.println("Button Pressed");

count++;

}

lastButton = button;

  

}
```


Button state checking (short and long press):

```
uint8_t count = 0;

uint8_t lastButton = 0;

int shortPress = 300;

int longPress = 800;

long pressedTime;

  

void setup() {

// put your setup code here, to run once:

Serial.begin(9600);

pinMode(2, INPUT);

  

}

  

void loop() {

// put your main code here, to run repeatedly:

long fsr_signal = analogRead(A0);

// Serial.println(fsr_signal);

  

int button = digitalRead(2);

  

if(button != lastButton){

Serial.println("Button Pressed");

if(button == HIGH){

pressedTime = millis();

}

if(button == LOW){

long heldTime = millis() - pressedTime;

Serial.print("Held Time: ");

Serial.println(heldTime);

if(heldTime <= shortPress){

Serial.println("Short Press");

}

else if (heldTime >= longPress) {

Serial.println("Long Press");

}

}

}

lastButton = button;

  

}
```


Analog threshold crossing
```
uint8_t count = 0;

uint8_t lastButton = 0;

int shortPress = 300;

int longPress = 800;

long pressedTime;

long threshold = 500;

long lastReading = 0;

  

void setup() {

// put your setup code here, to run once:

Serial.begin(9600);

pinMode(2, INPUT);

  

}

  

void loop() {

// put your main code here, to run repeatedly:

long fsr_signal = analogRead(A0);

// Serial.println(fsr_signal);

if(fsr_signal > threshold){

if(lastReading < threshold){

Serial.println("Threshold just crossed");

}

}

  

lastReading = fsr_signal;

}
```

Combining FSR, threshold, and peak to fade an LED:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/7c8fd535-110b-49ec-a35f-34695a00c4cd?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>


```
uint8_t count = 0;

uint8_t lastButton = 0;

int shortPress = 300;

int longPress = 800;

long pressedTime;

long threshold = 400;

long lastReading = 0;

long peak = 0;

float fade = 0;

  
  

void setup() {

// put your setup code here, to run once:

Serial.begin(9600);

pinMode(3, OUTPUT);

  

}

  

void loop() {

// put your main code here, to run repeatedly:

long fsr_signal = analogRead(A0);

// Serial.println(fsr_signal);

if(fsr_signal > threshold){

if(fsr_signal > peak){

peak = fsr_signal;

Serial.println("New Peak: ");

Serial.println(peak);

}

}

  

else if(fsr_signal <= threshold && peak > 0){

Serial.println("Below threshold, reset peak");

fade = map(peak,0,1023,0,200);

peak = 0;

}

if(fade > 0){

Serial.print("Fading: ");

Serial.println(fade);

analogWrite(3, fade);

fade-=.2;

}

}
```