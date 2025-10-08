

{PIC OF MOTOR ONLY ON HIGH}

{PIC OF MOTOR ALTERNATING}
```
const int transistorPin = 2; // connected to the base of the transistor

void setup() {

// set the transistor pin as output:

pinMode(transistorPin, OUTPUT);

}

void loop() {

digitalWrite(transistorPin, HIGH);

delay(1000);

digitalWrite(transistorPin, LOW);

delay(1000);

}
```

DC + Potentiometer working

```
const int transistorPin = 2; // connected to the base of the transistor

void setup() {

// set the transistor pin as output:

pinMode(transistorPin, OUTPUT);

}

void loop() {

// read the potentiometer:

int sensorValue = analogRead(A0);

// map the sensor value to a range from 0 - 255:

int outputValue = map(sensorValue, 0, 1023, 0, 255);

// use that to control the transistor:

analogWrite(transistorPin, outputValue);

}
```