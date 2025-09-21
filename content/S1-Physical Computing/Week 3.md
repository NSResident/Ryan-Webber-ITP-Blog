
Questions:
* Do you need a resistor for a potentiometer when using it in analog input

Additional Reading/Videos:

[High Current Loads](https://itp.nyu.edu/physcomp/videos/videos-transistors-and-motors/)


lab

Speaker and LED with potentiometer:

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


Before doing the next lab, I watched the video on transiters. From my understanding, it's a switch that uses an voltage as a signal to complete a circuit.

![[Pasted image 20250916163448.png]]

Small voltage goes through the base a switch, which passes the voltage from the collector to the emitter.

Speaker with transistor playing 2 tones:

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


