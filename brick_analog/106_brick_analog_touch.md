# #106 Touch Brick
<center>![](/img/100_analog/product/106.jpg)
<!--COLORME-->

## Overview
感圧センサーを使用したタッチセンサーBrickです。

I/Oピンより、感圧部分に加えられた力の大きさの変化をアナログ値で取得することができます。

## Connecting

### Arduino
アナログコネクタ(A0〜A5)のいずれかに接続します。

![](/img/100_analog/connect/106_touch_connect.jpg)

###Raspberry PI
アナログコネクタ(A0〜A7)のいずれかに接続します。

### IchigoJam
アナログ用コネクタ(IN2またはANA()で設定したコネクタ)のどれかに接続します。


## Support
|Arduino|RaspberryPI|IchigoJam|
|:--:|:--:|:--:|
|◯|◯|◯|

## Datasheet
| Document |
|:--|
| [Datasheet](http://interlinkelectronics.com/datasheets/Datasheet_FSR.pdf) |

## Schematic
![](/img/100_analog/schematic/106_touch.png)

## Sample Code
### for Arduino
A0コネクタに接続したTouch Brickの感圧によって、D2コネクタに接続したLED Brickを点灯/消灯させています。

```c
//
// FaBo Brick Sample
//
// #106 Touch Brick
//

#define buttonPin A0
#define ledPin 2

int buttonState = 0;

void setup() {
  pinMode(buttonPin, INPUT);
  pinMode(ledPin, OUTPUT);
}

void loop(){

  buttonState = digitalRead(buttonPin);

  if (buttonState == HIGH) {
    digitalWrite(ledPin, LOW);
  }
  else {
    digitalWrite(ledPin, HIGH);
  }
}
```

### for Raspberry PI
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

### for IchigoJam
#####注意<br>アナログはIN2のみで数値取得可能です。
デジタルの場合はIN(2)、アナログの場合がANA(2)とします。
```
100 'TOUCH_sample_program
110 CLS
120 LOCATE 10,8:PRINT "Digital =";IN(2)
130 LOCATE 10,9:PRINT "Analog  =";ANA(2);"  "
140 GOTO 120
```
加圧部分に力を加えると数値が変化します。<br>
指で挟んで力んでみてください。<br>
デジタル数値は、圧が強いと0に変化します。

### for Edison
A0コネクタに接続したTouch Brickの感圧によって、D2コネクタに接続したLED Brickを点灯/消灯させています。

```js
//
// FaBo Brick Sample
//
// #106 Touch Brick
//

//library
var m = require('mraa');

//pin setup
var myButton = new m.Gpio(14); //Touch sensor A0
var myLed    = new m.Gpio(2);  //LED D2

myButton.dir(m.DIR_IN);     //Touch sensor input
myLed.dir(m.DIR_OUT);       //LED output

//call loop function
loop();

function loop()
{

  if (myButton.read()){
    myLed.write(0);
  }
  else {
    myLed.write(1);
  }

  setTimeout(loop, 10);
}
```

## Parts
- 感圧センサー

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/106_touch
