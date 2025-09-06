### Reading:
[Electricity](https://itp.nyu.edu/physcomp/lessons/electronics/electricity-the-basics/) 
[Videos -  Electronic Tools](https://itp.nyu.edu/physcomp/videos/videos-tools/#Electronic_tools
[Lab: Electronics](https://itp.nyu.edu/physcomp/labs/electronics/)
[Lab: Components](https://itp.nyu.edu/physcomp/labs/components/)
[Lab: Breadboard](https://itp.nyu.edu/physcomp/labs/breadboard/)
[Lab Switches](https://itp.nyu.edu/physcomp/labs/switches/)

### Lessons Learned
* Electricity isn't as intimidating as I thought
* Always test your power supply and tools 
* Ask for help with concepts and experiment to solidify them (Thanks Cody)
* Check the circuit for any small mistakes BEFORE powering it on

### Outstanding questions
* None
 
### Goals Before Next Class
* Strip wires of different lengths for a cleaner board
* Something fun?

### Journey
First tried to use DC power, but ran into an issue where the power supply was 12V but my tool was reading 16.5V 

![](https://PCompPull.b-cdn.net/IMG_4552.jpeg)


![](https://PCompPull.b-cdn.net/IMG_4553.jpeg)


My first thought was a defective power supply and was a little nervous to run it through the lab since the regulator can only handle voltages between 5-15 so I brought it to the shop staff. They said it was more likely to be an issue with the measurement tool (which came up again later in the labs) and that it was safe to proceed, but to avoid the issue I ended up powering the board with the microcontroller instead of DC power. Making a note to try it again next week.

Shortly after, I got my first LED working:

![](https://PCompPull.b-cdn.net/IMG_4555.jpeg)

Afterwards I added a regulator and continued to add more wires to test my understanding of the board's layout and just electricity in general:

![](https://pcomppull.b-cdn.net/IMG_4558.jpeg)


Using the multimeter, I wanted to test the voltage before and after the regulator to make sure everything was working:

![](https://PCompPull.b-cdn.net/IMG_4563.jpeg)


![](https://pcomppull.b-cdn.net/IMG_4560.jpeg)



Afterwards I moved to switches, which I got working pretty quickly:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/a6432f5e-b4a6-470a-ae1a-57d7187f4dcf?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>

But the light was really dull, which made me realize I was still using the regulator to control the voltage.

Afterwards I got the 2 LED set-up working with a single switch:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/7402e8de-6917-4249-9161-75a714584152?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>



And a series with 2 switches:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/9a8a7724-1408-4818-91af-d701e9b9d11f?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>


And then 2 switches in parallel..... which failed:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/73ce540b-fca5-4175-939f-395ba079820a?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>

Clearly I was doing something wrong and my second attempt was closer but ultimately also failed:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/827a1cfa-d2b9-411a-b27e-8b8a28482a87?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>



I realized that the output of the first switch was never making it to the LED. After some cable swapping I got it to work:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/1f0726e9-ef2c-49cd-b19e-8499e1011ab7?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>

And extended it to 3 switches:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/0ea4bccd-27c7-41ea-8dc3-6478d68808b0?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>


Next was using the potentiometer, which.... failed at first:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/72fb0f76-883a-472f-a41a-a8184906d182?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>


Last on switches was making a custom switch. I decided to use my airpods and my laptop case, which, after a few frustrating attempts of getting the tape right, worked out:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/1acc10ef-5620-4c52-8380-22b9fae372e9?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>


But on realizing that I placed the cathode on the power side instead of ground. Swapping them immediately fixed the problem. Good note to always check the board before powering it up:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/e845e74f-4323-4e6d-9014-053c5b2abf81?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>


Later on I found out that there was an issue with the 12V power supplies from the shop (the same model that I used earlier) that caused them to output 16V. After they were collected and only real 12V power supplies were available, I went back and did the lab assignments that required 5V, such as setting up a series of LEDs:

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/46b75b8f-0583-4d89-80b4-982f843d7bd6?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>


(Low light due to different voltage requirements for the LEDs)

<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/62bc91f1-d258-46fa-9462-f9ba96da1278?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>

And parallel LEDs:
<div style="position:relative;padding-top:56.25%;"><iframe src="https://iframe.mediadelivery.net/embed/490829/d4708ff9-83eb-4510-abcb-df000078b939?autoplay=false&loop=true&muted=true&preload=true&responsive=true" loading="lazy" style="border:0;position:absolute;top:0;height:100%;width:100%;" allow="accelerometer;gyroscope;autoplay;encrypted-media;picture-in-picture;" allowfullscreen="true"></iframe></div>


All in all this was a pretty fun start to the class, and am definitely excited about connecting different sensors (and even different breadboards) together for future project class. 
