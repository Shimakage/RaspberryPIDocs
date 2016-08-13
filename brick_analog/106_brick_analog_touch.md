# #106 Touch Brick
<center>![](/img/100_analog/product/106.jpg)
<!--COLORME-->

## Overview
感圧センサーを使用したタッチセンサーBrickです。

I/Oピンより、感圧部分に加えられた力の大きさの変化をアナログ値で取得することができます。

## Connecting

### Arduino

アナログコネクタ(A0〜A7)のいずれかに接続します。

## Datasheet
| Document |
|:--|
| [Datasheet](http://interlinkelectronics.com/datasheets/Datasheet_FSR.pdf) |

## Schematic
![](/img/100_analog/schematic/106_touch.png)

## Sample Code

A0コネクタにTouchを接続して、GPIO4コネクタに接続したLED Brickの明るさ調節に使用しています。
```python
#!/usr/bin/env python
# coding: utf-8

#
# FaBo Brick Sample
#
# #106 Touch Brick
#

import RPi.GPIO as GPIO
import spidev
import time
import sys

# A0コネクタにTouchを接続
TOUCHPIN = 0

# GPIO4コネクタにLEDを接続
LEDPIN = 4

# GPIOポートを設定
GPIO.setwarnings(False)
GPIO.setmode( GPIO.BCM )
GPIO.setup( LEDPIN, GPIO.OUT )

# PWM/100Hzに設定
LED = GPIO.PWM(LEDPIN, 100)

# 初期化
LED.start(0)
spi = spidev.SpiDev()
spi.open(0,0)

def readadc(channel):
	adc = spi.xfer2([1,(8+channel)<<4,0])
	data = ((adc[1]&3) << 8) + adc[2]
	return data

def arduino_map(x, in_min, in_max, out_min, out_max):
	return (x - in_min) * (out_max - out_min) // (in_max - in_min) + out_min

if __name__ == '__main__':
	try:
		while True:
			data = readadc(TOUCHPIN)
			print("adc : {:8} ".format(data))
			value = arduino_map(data, 0, 1023, 100, 0)
			LED.ChangeDutyCycle(value)
			time.sleep( 0.01 )
	except KeyboardInterrupt:
		LED.stop()
		GPIO.cleanup()
		spi.close()
		sys.exit(0)
```

## 構成Parts
- 感圧センサー

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/106_touch
