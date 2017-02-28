# LidarLite

## Overview
Lidar Lite V2の連携方法

## Connecting

## Support
|Arduino|RaspberryPI|
|:--:|:--:|
|◯|◯|

## LidarLiteV2 Datasheet
|Document|
|--|
|[LidarLiteV2](https://cdn.sparkfun.com/datasheets/Sensors/Proximity/lidarlite2DS.pdf)|

## Schematic

## Libraryの設定

```
$ sudo apt-get install scons swig
$ git clone https://github.com/jgarff/rpi_ws281x.git
$ cd rpi_ws281x
$ scons
$ cd python
$ sudo python setup.py install
```

## Sample

```python
# coding: utf-8
import time
from neopixel import *

# LED strip configuration:
LED_COUNT = 12  # LEDの数.
LED_PIN = 18      # Color LED Brickの接続先.
LED_FREQ_HZ = 800000  # LED signal frequency in hertz (usually 800khz)
LED_DMA = 5 # DMA channel to use for generating signal (try 5)
LED_BRIGHTNESS = 100 # Set to 0 for darkest and 255 for brightest
LED_INVERT     = False   # True to invert the signal (when using NPN transistor level shift)

# Color sensor
c = Adafruit_NeoPixel(LED_COUNT, LED_PIN, LED_FREQ_HZ, LED_DMA, LED_INVERT, LED_BRIGHTNESS)
c.begin()

m = 0
r = 0
b = 255

while True:
    for i in range(LED_COUNT):
        if m%12 == i:
            c.setPixelColor(i, Color(100,r,b)) 
        else:
            c.setPixelColor(i, Color(0,0,0))
    c.show()
    m += 1
    r += 5
    b -= 5
    if r > 255:
        r = 0
    if b < 0:
        b = 255
    time.sleep(0.1)

```

