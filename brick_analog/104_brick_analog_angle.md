# #104 Angle Brick

<center>![](/img/100_analog/product/104.jpg)
<!--COLORME-->

## Overview
ボリューム抵抗を使ったBrickです。

I/Oピンからアナログ値を取得することができます。

LED Brickの明るさを調節する際などに使用します。

## Connecting

アナログコネクタ(A0〜A7)のいずれかに接続します。

##Schematic
![](/img/100_analog/schematic/104_angle.png)

## Sample Code

A0コネクタにAngle Brickを接続して、D3コネクタに接続したLED Brickの明るさ調節に使用しています。

```python
#!/usr/bin/env python
# coding: utf-8

#
# FaBo Brick Sample
#
# #104 Angle Brick
#

import RPi.GPIO as GPIO
import spidev
import time
import sys

# A0コネクタにAngleを接続
ANGLEPIN = 0

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
			data = readadc(ANGLEPIN)
			print("adc : {:8} ".format(data))
			value = arduino_map(data, 0, 1023, 0, 100)
			LED.ChangeDutyCycle(value)
			time.sleep( 0.01 )
	except KeyboardInterrupt:
		LED.stop()
		GPIO.cleanup()
		spi.close()
		sys.exit(0)
```

## 構成Parts
- ボリューム抵抗器A 10k

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/104_angle
