

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
