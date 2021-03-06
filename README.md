# esp8266_micropython
###### MQTT light component for Home Assistant with ws2812 LED's, and an esp8266 (Nodemcu) flashed with MicroPython

##Features:
1. Control your MQTT Lights from Home Assistant (Both mqtt\_light, and mqtt\_json\_light supported.)
2. When a color is picked, the relative brightness of that color is set on the brightness slider
3. When a color is picked, and the brightness slider moved, the current color moves as well


###Short and Sweet
All you  need is the no-webrepl.bin and mlight.py or mjlight.py. flash no-webrepl.bin to the board, and copy mlight.py or mjlight.py to the board, named as main.py. The configuration for the component in home assistant is int the scripts.


###Setup the Board:
I used [esptool.py](https://github.com/themadinventor/esptool) to flash the micropython to the board

This is the version of [MicroPython](https://github.com/micropython/micropython/releases/tag/v1.8.5) I used. I'd say just use the most recent one.
Flashing the board is pretty straight forward, the docs for MicroPython tell you what settings you'd need to change if any.


###Connecting to the Board:

I used Adafruits [ampy](https://github.com/adafruit/ampy) to put all my files onto the board.

Check out this [tutorial](https://home-assistant.io/blog/2016/07/28/esp8266-and-micropython-part1/) as well.


###Issues:

When the nodemcu boots up with MicroPython, it runs "boot.py" and then "main.py". If you run the main function of "mlight.py" in "main.py", you'll get stuck in an infinite loop, since its going to be waiting for a publish to a mqtt topic it subscribed to. In order to get rid of this, I just flashed "mlight.py" onto the board, typed "screen /dev/tty.SLAB_USBtoUART 115200" and automatically started and stopped the script (helpful for debugging). Another option would be to just add a break, when a certain topic is called. then you'd be able to access the board via ampy, or screen or whatever you're using.


###Bugs:

When setting the color in Home Assistant's dev tools, if you send something like "{"color":"red"}" it while go to white then if you press it again, it will go to red. Not sure why.

Brightness of the colors. If you have a color on and then change the brightness, it turns to white.

###Purchase List:
[NodeMCU v2](http://www.ebay.com/itm/like/251980906073?lpid=82&chn=ps&ul_noapp=true)
	any NodeMCU should be good, but make sure its the [official kind](http://frightanic.com/iot/comparison-of-esp8266-nodemcu-development-boards/)

[WS2812](http://www.aliexpress.com/w/wholesale-ws2812.html)
    any of these work, you'll need a power supply, 5V,
    if it's a small strip you might be able to do what I did and cut an old phone changer and solder to female ends of wires to them and connect it with that.


###Examples and their functions:

* police.py: simple police lights example
* wipe.py: wipe color across the strip
* rainbow.py: show rainbow on LEDs, SEPERATION is how far the next LED is from the previous in terms of color
* strobe.py: a strobe light
* on\_off.py: type the led index you want to turn on, and then turn it on. and then off again
* fire.py: fire flicker on led strip. flicker_intensity gets weird at high numbers since there is no random.randint().  we use getrandbits() :(
* smooth.py: smooth transistions from one color to the next
* hsv\_to\_rgb.py: hsv to rgb simple script in own file. good for rainbow.


###TODO:
* Add json support! I will be working on this in the next week or so. This will be the mjlight.py file.
