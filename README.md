# esp8266 Honeywell smart fan
Making a Honeywell HT354E4 smart using ESPHome.

Hello all and welcome to my latest project, taking the rather decent yet ‘dumb’ Honeywell HT354E4 quietset desk fan smart, and fully integrated with Home assistant.

Here is the fan in question, it’s a very good fan, it moves a lot of air and is relatively quiet. Currently retailing on amazon.co.uk for £46, however during the summer prices can and will rise.
https://www.amazon.co.uk/gp/product/B00T7L4LUS/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1

In addition to the fan, you’ll also need.
1x ESP device, I used a Wemos/ Lolin D1 mini clone, but any ESP device should work providing you adapt the yaml appropriately.
3x mosfet pwm modules, I used these https://www.amazon.co.uk/gp/product/B07VRCXGFY/ref=ppx_yo_dt_b_asin_title_o07_s00?ie=UTF8&psc=1
2x 3 wire connectors, just to make life easier assembling the fan, I used these https://www.amazon.co.uk/gp/product/B01DF0UL8C/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1
A few wires, and a soldering iron.

Optionally a hot glue gun, not strictly required but I find very useful for these kinds of projects.
I of course used a multimeter too to determine pins and voltage, but I’ve done the hard work for you, see the picture of everywhere you need to solder!

Fantastically enough the internal circuit board in the Honeywell fan run at 5V DC, and supplies that power even when the fan is ‘off’, what a happy coincidence :)
So basically, what we’re doing with this code is simulating button presses on the fan control. As each button basically is just a momentary switch to ground, we’ll do this by momentarily grounding the appropriate point on the circuit board using our MOSFTET PWM modules, thus simulating a button press. I found that 75ms was the quickest grounding time I could get away with, at 50ms the fan wouldn’t register a button press!

We’ll use a couple of variables internally to keep track of our fan speed, power on status, and oscillation status, this is required as we’re always toggling, and to change fan speed say from 2 to 4 it would require simulating 2 button presses, but don’t worry too much about this its all taken care of in the yaml :)

I took this approach as its pretty straight forward electronics and coding, the limitation being if the physical buttons on the fan are pressed the smart side of things doesn’t know about it, and things can get out of sync, re-syncing would involve physical pressing the fan off button, then changing the speed in HA, this would set everything back, this is a limitation I’m perfectly ok with, as once its smart I can’t imagine I’d ever use the physical buttons again.

Now it may be possible to design this smarter so physical button presses on the fan can be understood by the ESP device, I suspect this could be done by understanding the logic chip better, however it looked like too much faff and the logic chip is all 5V where as our ESP devices are 3.3V (on the GPIO pins) I didn’t bother digging this far.

ESP wise I used GND, 5V, D6 for oscillate button, D5 for speed button, and D2 for power on/off button, should however work with any pins you want, just remember to update the yaml appropriately.

apologies for some of the unconventional wire colour used, I've labled up the pictures as best I could, hopefully its clear.

Software wise we’re going to use the fantastic ESPHome project https://esphome.io/

To correctly support 4 speeds on a fan you will need at least the following versions

Home assistant: version 2021.4.0b3
EspHome: 1.17.0b1
Yes, that’s right both are in BETA at time of writing, this is cutting edge stuff ;)

Recommended steps.
1.	Look at the pictures
2.	Get the hardware
3.	Open the fan
4.	get soldering onto the circuit board.
5.	Download/edit the yaml & flash your esp
6.	Wire everything up on the bench
7.	Test
8.	Get gluing & assemble!

Enjoy, keep safe, particularly when testing on the bench, there is mains voltage here!

