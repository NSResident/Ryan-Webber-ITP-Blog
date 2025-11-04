

![[Week 4 - Midterm Idea 2025-09-30 09.37.29.excalidraw]]

![](https://PCompPull.b-cdn.net/Midterm/Midterm%20Idea.png)


Next steps:
- [ ] Flow of components
- [ ] Schedule 
- [ ] Circuit design: https://www.tinkercad.com/things/9LoUg7DbK2P-funky-rottis-vihelmo 
- [ ] Mozzi library
- [ ] User flow - state diagram
- [ ] BOM 
- [ ] Solder rangefinder
- [ ] Test photosensor
- [ ] Week 1 - Code done?'



Code
board - small, solderless 
[battery](https://www.adafruit.com/product/1578)
[speaker](https://www.amazon.com/Dweii-Loundspeaker-JST-PH2-0-Electronic-Advertising/dp/B0B4D1BN4F/ref=sr_1_1?dib=eyJ2IjoiMSJ9.FxuzjXkPr41Aq06cVivzUVoGDDhpwxNzGDLpFdkIb3d8I98mDABlBHCaloS0h791LFoSd4_Z21LeGgf5KjU6flSiKg_kDVKO3Uhs5vzlr4blc3K178qtMqoylW3pxp6hc85UyPpoztQTJ9QAFzuqZSflzqd-wEDCxD5YoI5uk9GMbvEeUW5XJPEWbdpR5SIbR3nvTQbJCB4-4l_rZJksgfejCTZpmg3pvCNXEQToYuw.pvvZzVeFsRGeqcG6svKDv2rytzbXcXFhM2YT9ESUMIg&dib_tag=se&keywords=arduino%2Bspeaker&qid=1759522796&sr=8-1&th=1)


Time of Flight Example not working

```
#include "Adafruit_VL53L0X.h"

  

Adafruit_VL53L0X lox = Adafruit_VL53L0X();

  

void setup() {

Serial.begin(115200);

  

// wait until serial port opens for native USB devices

while (! Serial) {

delay(1);

}

Serial.println("Adafruit VL53L0X test");

if (!lox.begin()) {

Serial.println(F("Failed to boot VL53L0X"));

while(1);

}

// power

Serial.println(F("VL53L0X API Simple Ranging example\n\n"));

}

  
  

void loop() {

VL53L0X_RangingMeasurementData_t measure;

Serial.print("Reading a measurement... ");

lox.rangingTest(&measure, false); // pass in 'true' to get debug data printout!

  

if (measure.RangeStatus != 4) { // phase failures have incorrect data

Serial.print("Distance (mm): "); Serial.println(measure.RangeMilliMeter);

} else {

Serial.println(" out of range ");

}

delay(100);

}
```


Class 10/7 

* Check voltage of lion batteries because the arduino has a minimum voltage 
* Tone library instead of mozzi 


[Attaching the arduino to the box](https://www.thistothat.com/)

# Midterm

* Size of creature
* Testing out sensors and placing them 
* Main interaction is different sounds in the dark
* Bonus: time of flight sensor for distancing sensing

# Final

- Multiple creatures 
- Media Commons Audio Studio Setup (spatial audio)
- Communicate through bluetooth 
- Light, heat, [touch](https://www.adafruit.com/product/1361?srsltid=AfmBOopG7x-x0YiwYF4Py7ysQk-7cbpnYoiQxLrDfhKQtWv7kHozwWDK) sensor 


	[Voltage Regulator Datasheet](chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://www.tme.eu/Document/819c9aab5a8ac10f3ac6d1ec8e06fcd3/lm7806.pdf)



Personalities
- Dark and light personalities 
- Depends on which one you do first




Attempt 1:

```
  

#define MOZZI_CONTROL_RATE 64 // Hz, powers of 2 are most reliable; 64 Hz is actually the default, but shown here, for clarity

#include <Mozzi.h>

#include <Oscil.h> // oscillator template

#include <tables/sin2048_int8.h> // sine table for oscillator

  
  

const int FREQ_1 = 440; // C#4

  

Oscil<SIN2048_NUM_CELLS, MOZZI_AUDIO_RATE> aSin(SIN2048_DATA);

  

unsigned long startTime;

  
  

//Tuple {start, end}

unsigned long sequencer_intervals[2] = { 0, 4000 };

unsigned int interval_point = 0;

  

enum Effect { FADE_UP_AMP,

FADE_DOWN_AMP };

Effect sequencer_effect[1] = { FADE_UP_AMP };

unsigned int effect_point = 0;

  

enum EndCase { LOOP,

END,

REVERSE };

EndCase currentEnd = LOOP;

  

//EFFECT GLOBALS

const int MAX_SCALE = 200;

enum State { PLAYING, PAUSING };

State currentState = PLAYING;

// int PAUSE_DURATION = 1000;

int global_scale = 0;

  

void setup() {

// put your setup code here, to run once:

Serial.begin(9600);

if (sizeof(sequencer_effect) < 1) {

Serial.println("Sequencer Effect array empty");

return;

}

if (sizeof(sequencer_intervals) != 2 * (sizeof(sequencer_effect))) {

Serial.println("Sequencer Interval != 2*Sequencer Effect");

return;

}

  

startMozzi(CONTROL_RATE);

  

// Set the frequencies for all three oscillators

aSin.setFreq(FREQ_1);

  

// Initialize the timer

startTime = millis();

}

  

void updateControl() {

  

//Check for sensor interrupt

unsigned long currentTime = millis();

unsigned long elapsedTime = currentTime - startTime;

  

unsigned long start = sequencer_intervals[interval_point];

unsigned long end = sequencer_intervals[interval_point + 1];

unsigned long duration = end - start;

  

//Still in an effect

if (elapsedTime < duration) {

Serial.println("In an effect");

Effect currentEffect = sequencer_effect[effect_point];

switch (currentEffect) {

case FADE_UP_AMP:

{

Serial.println("FADE UP AMP");

global_scale = fadeUpAMP(elapsedTime, duration);

}

  

case FADE_DOWN_AMP:

{

}

}

}

  

//Current effect has ended. Move on to next effect

else if (sizeof(sequencer_intervals) > interval_point + 2) {

startTime = millis();

interval_point += 2;

effect_point++;

}

  

//All effects have ended, evaluate end case

else {

switch (currentEnd) {

case LOOP:

{

startTime = millis();

interval_point = 0;

effect_point = 0;

}

}

}

}

  

int fadeUpAMP(long elapsedTime, long duration) {

int currentScale = 0; // The 0-255 scale value

int PAUSE_DURATION = duration - 1000;

duration-=1000;

switch (currentState) {

case PLAYING:

{

if (elapsedTime < duration) {

// Linearly map elapsed time (0 to PLAY_DURATION) to amplitude scale (0 to MAX_SCALE)

currentScale = map(elapsedTime, 0, duration, 0, MAX_SCALE);

} else {

// Transition to PAUSING state

currentState = PAUSING;

currentScale = 0; // Immediate silence

}

break;

}

  

case PAUSING:

{

// if (elapsedTime >= PAUSE_DURATION) {

// }

currentScale = 0; // Keep amplitude at 0

break;

}

}

Serial.println(currentScale);

return currentScale;

}

  
  

AudioOutput_t updateAudio() {

  

// The oscillator next() returns a value from -128 to 127 (INT8_t).

// 1. Calculate the scaled output of each voice:

// - Cast to long for safe multiplication.

// - Multiply by the scale factor (0-255).

// - Right-shift by 8 (equivalent to dividing by 256) to scale

// the 16-bit intermediate back down to the 8-bit output range.

// long sample1 = ((long)aSin.next() * MAX_SCALE) >> 8;

int sample1 = (int)(((long)aSin.next() * global_scale) >> 8);

  

return MonoOutput::from8Bit(aSin.next());

}

  
  

void loop() {

// put your main code here, to run repeatedly:

audioHook();

}
```


Amp is scaling but not getting modified at the end.

1st Mistake: sizeof not returning what i think
Supposed to return 2 for sequencer_interval and 1 for sequencer_effect 
Returning 8 for sequencer_interval and 2 for sequencer_effecet

https://stackoverflow.com/questions/37538/how-do-i-determine-the-size-of-my-array-in-c

Sizeof counts bytes not the individual elements


after new code, it now plays
```
  

#define MOZZI_CONTROL_RATE 64 // Hz, powers of 2 are most reliable; 64 Hz is actually the default, but shown here, for clarity

#include <Mozzi.h>

#include <Oscil.h> // oscillator template

#include <tables/sin2048_int8.h> // sine table for oscillator

  
  

const int FREQ_1 = 440; // C#4

  

Oscil<SIN2048_NUM_CELLS, MOZZI_AUDIO_RATE> aSin(SIN2048_DATA);

  

unsigned long startTime;

  
  

//Tuple {start, end}

unsigned long sequencer_intervals[2] = { 0, 4000 };

unsigned int interval_point = 0;

  

enum Effect { FADE_UP_AMP,

FADE_DOWN_AMP };

Effect sequencer_effect[1] = { FADE_UP_AMP };

unsigned int effect_point = 0;

  
  

size_t effect_count = (sizeof(sequencer_effect)/sizeof(sequencer_effect[0]) );

size_t interval_count = (sizeof(sequencer_intervals)/sizeof(sequencer_intervals[0]));

  
  

//How to end

enum EndCase { LOOP,

END,

REVERSE };

EndCase currentEnd = LOOP;

  

//EFFECT GLOBALS

const int MAX_SCALE = 200;

enum State { PLAYING,

PAUSING };

State currentState = PLAYING;

// int PAUSE_DURATION = 1000;

int global_scale = 0;

  
  

void setup() {

// put your setup code here, to run once:

Serial.begin(9600);

if (effect_count < 1) {

Serial.println("Sequencer Effect array empty");

return;

}

if (interval_count != 2*effect_count) {

Serial.println("Sequencer Interval != 2*Sequencer Effect");

return;

}

  

startMozzi(CONTROL_RATE);

  

// Set the frequencies for all three oscillators

aSin.setFreq(FREQ_1);

  

// Initialize the timer

startTime = millis();

}

  

void updateControl() {

  

//Check for sensor interrupt

unsigned long currentTime = millis();

unsigned long elapsedTime = currentTime - startTime;

  

unsigned long start = sequencer_intervals[interval_point];

unsigned long end = sequencer_intervals[interval_point + 1];

unsigned long duration = end - start;

  

//Still in an effect

if (elapsedTime < duration) {

Effect currentEffect = sequencer_effect[effect_point];

switch (currentEffect) {

case FADE_UP_AMP:

{

global_scale = fadeUpAMP(elapsedTime, duration);

}

  

case FADE_DOWN_AMP:

{

}

}

}

  

//Current effect has ended. Move on to next effect

else if (sizeof(sequencer_intervals) > interval_point + 2) {

startTime = millis();

interval_point += 2;

effect_point++;

}

  

//All effects have ended, evaluate end case

else {

switch (currentEnd) {

case LOOP:

{

startTime = millis();

interval_point = 0;

effect_point = 0;

}

}

}

}

  

int fadeUpAMP(long elapsedTime, long duration) {

int currentScale = 0; // The 0-255 scale value

int PAUSE_DURATION = duration - 1000;

duration -= 1000;

switch (currentState) {

case PLAYING:

{

if (elapsedTime < duration) {

// Linearly map elapsed time (0 to PLAY_DURATION) to amplitude scale (0 to MAX_SCALE)

currentScale = map(elapsedTime, 0, duration, 0, MAX_SCALE);

} else {

// Transition to PAUSING state

currentState = PAUSING;

currentScale = 0; // Immediate silence

}

break;

}

  

case PAUSING:

{

// if (elapsedTime >= PAUSE_DURATION) {

// }

currentScale = 0; // Keep amplitude at 0

break;

}

}

return currentScale;

}

  
  

AudioOutput_t updateAudio() {

  

// The oscillator next() returns a value from -128 to 127 (INT8_t).

// 1. Calculate the scaled output of each voice:

// - Cast to long for safe multiplication.

// - Multiply by the scale factor (0-255).

// - Right-shift by 8 (equivalent to dividing by 256) to scale

// the 16-bit intermediate back down to the 8-bit output range.

// long sample1 = ((long)aSin.next() * MAX_SCALE) >> 8;

int sample1 = (int)(((long)aSin.next() * global_scale) >> 8);

  

return MonoOutput::from8Bit(aSin.next());

}

  
  

void loop() {

// put your main code here, to run repeatedly:

audioHook();

}
```


But now it doesn't loop the effect, it plays the effect and then holds it forever. Looking at the code, I realized I didnt do things
* Update sizeOf usage everywhere 
* Reset the globalScale for Amp back to 0

Also I'll turn the volume to make it less annoying to test

That still didnt, work but I also noticed that I forgot to update the currentState to playing agian. I'll also add a resistor to the circuit to reduce the volume

After adding some logging, the looping logic is definitely working but for some reason the audio isnt getting paused again.

After paying more attention to why the amplitude was not increasing, I realized I was not playing the modified oscilator, but the base one. This was the culprit

```
long sample1 = ((long)aSin.next() * global_scale) >> 8;
return MonoOutput::from8Bit(aSin.next());
```

after changing it to the below, it worked as intended: 

```
long sample1 = ((long)aSin.next() * global_scale) >> 8;
return MonoOutput::from8Bit(sample1);
```


Now we'll try to add a VIbrato effect in sequence 

```
  

#define MOZZI_CONTROL_RATE 64 // Hz, powers of 2 are most reliable; 64 Hz is actually the default, but shown here, for clarity

#include <Mozzi.h>

#include <Oscil.h> // oscillator template

#include <tables/sin2048_int8.h> // sine table for oscillator

  

//For Vibrato

#include <tables/cos2048_int8.h> // table for Oscils to play

#include <mozzi_midi.h> // for mtof

//#include <mozzi_fixmath.h>

#include <FixMath.h> // Fixed point arithmetics

  

const int FREQ_1 = 440; // C#4

  

Oscil<SIN2048_NUM_CELLS, MOZZI_AUDIO_RATE> aSin(SIN2048_DATA);

  

Oscil<COS2048_NUM_CELLS, MOZZI_AUDIO_RATE> aCos(COS2048_DATA);

Oscil<COS2048_NUM_CELLS, MOZZI_AUDIO_RATE> aVibrato(COS2048_DATA);

const UFix<0,8> intensity = 0.5; // amplitude of the phase modulation

  
  

unsigned long startTime;

  

//Tuple {start, end}

unsigned long sequencer_intervals[4] = { 0, 4000, 0, 2000 };

unsigned int interval_point = 0;

  

enum Effect { FADE_UP_AMP,

FADE_DOWN_AMP, VIBRATO };

Effect sequencer_effect[2] = { FADE_UP_AMP, VIBRATO };

unsigned int effect_point = 0;

  
  

const size_t effect_count = (sizeof(sequencer_effect)/sizeof(sequencer_effect[0]) );

const size_t interval_count = (sizeof(sequencer_intervals)/sizeof(sequencer_intervals[0]));

  
  

//How to end

enum EndCase { LOOP,

END,

REVERSE };

EndCase currentEnd = LOOP;

  

//EFFECT GLOBALS

const int MAX_SCALE = 100;

enum State { PLAYING,

PAUSING };

State currentState = PLAYING;

// int PAUSE_DURATION = 1000;

int global_scale = 0;

  
  

void setup() {

// put your setup code here, to run once:

Serial.begin(9600);

if (effect_count < 1) {

Serial.println("Sequencer Effect array empty");

return;

}

if (interval_count != 2*effect_count) {

Serial.println("Sequencer Interval != 2*Sequencer Effect");

return;

}

  

startMozzi(CONTROL_RATE);

// Set the frequencies for all three oscillators

aSin.setFreq(FREQ_1);

aCos.setFreq(mtof(84.f));

aVibrato.setFreq(15.f);

// Initialize the timer

startTime = millis();

}

  

void updateControl() {

  

//Check for sensor interrupt

unsigned long currentTime = millis();

unsigned long elapsedTime = currentTime - startTime;

  

unsigned long start = sequencer_intervals[interval_point];

unsigned long end = sequencer_intervals[interval_point + 1];

unsigned long duration = end - start;

  

//Still in an effect

if (elapsedTime < duration) {

Effect currentEffect = sequencer_effect[effect_point];

switch (currentEffect) {

case FADE_UP_AMP:

{

global_scale = fadeUpAMP(elapsedTime, duration);

break;

}

case VIBRATO:

{

break;

}

}

}

  

//Current effect has ended, evaluate end case

else{

currentState = PLAYING;

//Current effect has ended. Move on to next effect

if (interval_count > interval_point + 2) {

Serial.println("IN NEXT EFFECT CASE");

startTime = millis();

interval_point += 2;

effect_point++;

}

else {

switch (currentEnd) {

case LOOP:

{

// Serial.println("IN LOOP CASE");

startTime = millis();

interval_point = 0;

effect_point = 0;

break;

}

}

}

}

}

  

int fadeUpAMP(unsigned long elapsedTime, unsigned long duration) {

int currentScale = 0; // The 0-255 scale value

// int PAUSE_DURATION = duration - 1000;

duration -= 1000;

switch (currentState) {

case PLAYING:

{

if (elapsedTime < duration) {

// Linearly map elapsed time (0 to PLAY_DURATION) to amplitude scale (0 to MAX_SCALE)

currentScale = map(elapsedTime, 0, duration, 100, MAX_SCALE);

} else {

// Transition to PAUSING state

currentState = PAUSING;

currentScale = 0; // Immediate silence

}

break;

}

  

case PAUSING:

{

// if (elapsedTime >= PAUSE_DURATION) {

// }

// Serial.print("Duration: ");

// Serial.println(duration);

currentScale = 0; // Keep amplitude at 0

break;

}

}

// Serial.print("Current Scale: ");

// Serial.println(currentScale);

return currentScale;

}

  
  
  

AudioOutput_t updateAudio() {

  

// The oscillator next() returns a value from -128 to 127 (INT8_t).

// 1. Calculate the scaled output of each voice:

// - Cast to long for safe multiplication.

// - Multiply by the scale factor (0-255).

// - Right-shift by 8 (equivalent to dividing by 256) to scale

// the 16-bit intermediate back down to the 8-bit output range.

// long sample1 = ((long)aSin.next() * MAX_SCALE) >> 8;

long sample1 = ((long)aSin.next() * global_scale) >> 8;

  

auto vibrato = intensity * toSFraction(aVibrato.next()); // Oscils in Mozzi are 8bits: 7bits of signal plus one bit of sign, so what they fit into a SFixMath<0,7> which is a signed fixMath type with 7 bits of value.

return !effect_point ? MonoOutput::from8Bit(sample1) : MonoOutput::from8Bit(aCos.phMod(vibrato));

}

  
  

void loop() {

// put your main code here, to run repeatedly:

audioHook();

}
```

After a small array length update, it worked! Sequenceer logic is fully functional then
Now lets play with the vibrato intensity and frequency. Since we are using floats to control vibrator, we'll need a new mapping function. After some googling, I realize the code for the map function is very simple. Changing all the types to float should do the trick:

```
float mapf(float x, float in_min, float in_max, float out_min, float out_max)

{
	return (x - in_min) * (out_max - out_min) / (in_max - in_min) + out_min;
}
```

```
intensity = mapf(elapsedTime, 0, duration, .2, .9);
```


To make this use an LFO isntead of a linear mapping, I first tried to use the sinf function and adjust the frequency manually. This turned out to be way more complicated than I needed:

```
// float result = sinf((float)elapsedTime);
```

If I wanted to change the frequency, I would have to break down the x is in sin(x) to sin(2.0 * PI * frequency_hz * time). 
But, since Mozzi already has built in Oscillators, I just had to make a new sin osciliator using the library and control the frequency from there.

```
Oscil<SIN2048_NUM_CELLS, MOZZI_AUDIO_RATE> sinIntensityOsc(SIN2048_DATA);
sinIntensityOsc.setFreq(.1f);
...

float result = (float)(sinIntensityOsc.next());
```


Putting the range sensor and mozzi, together, we get a strange result where the note doesnt remain chainged while the hand is detected. I get a weird chopping sound:

```
if (!sensor.isRangeComplete())

tofVal = -1;

else

tofVal = sensor.readRangeResult();

// if it's with the max distance:

// Serial.println(tofVal);

// bool lightReact = lightTrigger(lightVal);

// bool tofReact = tofTrigger(tofVal);

bool tofReact = false;

if (tofVal > 100 and tofVal < 500){

tofReact = true;

}

bool lightReact = true;

// //Bright with someone close

if(lightReact && tofReact){

freq_scale = 440;

}

  

//Bright with someone far

else if (lightReact && !tofReact){

freq_scale = 0;

}


....

aSin.setFreq(MAIN_SIN_FREQ+freq_scale);
```


My first guess is because the polling rate of the control loop is lower than the audio loop, MUCH lower. So the value isnt holding long. However, placing the setFreq in the control loop didn't improve it.

[After more research](https://groups.google.com/g/mozzi-users/c/eQosgqZAIn4), it was definitely the polling rate of the sensor in the control loop. Reading from the ToF sensor is a blocking call and in an asynchronous system like Mozzi, it's way too slow for the parameter changes to propogate properly to the audio loop. Reorienting, I decided I needed to do a few things:
* Write a much simpler version of the interaction to have something working without the ToF sensor
* Figure out the best way to use the ToF sensor without highly accurate real time interaction (maybe as a one way state switch?)
* If time permits, use a scheduler for the ToF sensor that would allow asynchronous distance reading

Mozzi has a built in analog read that seems a bit more asynchronous and attuned to real time readings, so focusing first different behaviors with the photoresistor and hardcoding the the functionality led me to this rewrite:

```
#define MOZZI_CONTROL_RATE 64 // Hz, powers of 2 are most reliable

#include <Mozzi.h>

#include <tables/sin2048_int8.h> // sine table for oscillator

#include <Oscil.h>

#include <tables/cos2048_int8.h> // table for Oscils to play

#include <mozzi_midi.h> // for mtof

//#include <mozzi_fixmath.h>

#include <FixMath.h> // Fixed point arithmetics

  

const int MAIN_SIN_FREQ = 440; // C#4

const float MAIN_COS_FREQ = 83.f;

  

Oscil<SIN2048_NUM_CELLS, MOZZI_AUDIO_RATE> aSin(SIN2048_DATA);

  

Oscil<COS2048_NUM_CELLS, MOZZI_AUDIO_RATE> aCos(COS2048_DATA);

Oscil<COS2048_NUM_CELLS, MOZZI_AUDIO_RATE> aVibrato(COS2048_DATA);

Oscil<SIN2048_NUM_CELLS, MOZZI_AUDIO_RATE> sinIntensityOsc(SIN2048_DATA);

  

UFix<0,8> intensity = 0.2; // amplitude of the phase modulation

unsigned long startTime = 0;

unsigned long endTime = 0;

unsigned long duration = 0;

unsigned long pauseDuration = 0;

  

unsigned int global_scale = 255;

  

inline float mapf(float x, float in_min, float in_max, float out_min, float out_max)

{

return (x - in_min) * (out_max - out_min) / (in_max - in_min) + out_min;

}

  

enum Behavior {DARK_ALONE, LIGHT_ALONE, LIGHT_TGT};

Behavior behavior = DARK_ALONE;

//Sensor Info

const unsigned int photoSensor = 6;

  

enum State { PLAYING,

PAUSING };

extern State currentState = PLAYING;

  

void setup() {

// put your setup code here, to run once:

Serial.begin(9600);

mozziInit();

}

  

void mozziInit(){

startMozzi(CONTROL_RATE);

// Set the frequencies for all three oscillators

aSin.setFreq(MAIN_SIN_FREQ);

aCos.setFreq(mtof(MAIN_COS_FREQ));

aVibrato.setFreq(5.f);

sinIntensityOsc.setFreq(.1f);

startTime = millis();

duration = random(3000, 5001);

endTime = startTime + duration;

pauseDuration = random(1000, 3001);

}

  

void updateControl(){

long photoVal = mozziAnalogRead(photoSensor);

if(photoVal > 300){

behavior = LIGHT_ALONE;

}

else{

behavior = DARK_ALONE;

}

  

switch(behavior){

case DARK_ALONE:

{

float result = (float)(sinIntensityOsc.next());

intensity = mapf(result,-1.0,1.0, .2, .9);

break;

}

case LIGHT_ALONE:

{

break;

}

case LIGHT_TGT: {

  

}

}

global_scale = playAndHold();

}

  

//Random play and hold times

unsigned int playAndHold(){

  

long currentTime = millis();

long elapsedTime = currentTime - startTime;

unsigned int currentScale = 0;

switch (currentState) {

case PLAYING:

{

if (elapsedTime < duration) {

// Linearly map elapsed time (0 to PLAY_DURATION) to amplitude scale (0 to MAX_SCALE)

currentScale = 255;

} else {

// Transition to PAUSING state

currentState = PAUSING;

currentScale = 0; // Immediate silence

}

break;

}

  

case PAUSING:

{

if( !(elapsedTime > pauseDuration + duration)){

currentScale = 0; // Keep amplitude at 0

break;

}

else{

startTime = millis();

duration = random(3000, 5001);

endTime = startTime + duration;

pauseDuration = random(1000, 3001);

currentScale = 255;

currentState = PLAYING;

}

}

}

return currentScale;

}

  

AudioOutput updateAudio(){

auto vibrato = intensity * toSFraction(aVibrato.next());

long scaled_audio = ((long)aSin.next() * global_scale) >> 8;

long scaled_audio_vibrato = ((long)aCos.phMod(vibrato)*global_scale) >>8;

return behavior ? MonoOutput::from8Bit(scaled_audio) : MonoOutput::from8Bit(scaled_audio_vibrato);

  

}

  

void loop() {

// put your main code here, to run repeatedly:

audioHook();

}
```


Now playing around with different signal modifiers namely:
* Smooth
* Vibrato 
* Delay

Having a hard time getting a smooth start to my sounds but now all the sensors are working. Tomorrow I want to try customizing the sounds, making them cleaner, and, if I have time, using event delays in my code instead of the play pause logic