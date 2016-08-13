# #101 LED Brick

<center>![](/img/100_analog/product/101.jpg)
<!--COLORME-->

## Overview
LEDのBrickです。発光色は5色（青・緑・赤・白・黄）あります。Lチカのおともにもどうぞ。

※購入時は色の間違いにご注意ください。

## Connecting

GPIOコネクタのいずれかに接続します。

## Schematic
![](/img/100_analog/schematic/101_led.png)

## Sample Code

GPIO4コネクタにLED Brickを接続し、一定時間ごとに点灯/消灯させています。

```python
# coding: utf-8
#
# FaBo Brick Sample
#
# brick_analog_led
#

import RPi.GPIO as GPIO
import time

LEDPIN = 4

GPIO.setwarnings(False)
GPIO.setmode( GPIO.BCM )
GPIO.setup( LEDPIN, GPIO.OUT )

while 1:
	GPIO.output( LEDPIN, True )
	time.sleep( 1.0 )
	GPIO.output( LEDPIN, False )
	time.sleep( 1.0 )
```

## 構成Parts
- 5mm LED(各色)

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/101_led
