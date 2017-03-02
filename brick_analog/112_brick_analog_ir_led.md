# #112 IR LED Brick

<center>![](/img/100_analog/product/112.jpg)
<!--COLORME-->

## Overview
赤外線LEDを使ったBrickです。

I/Oピンから赤外線LEDをON/OFFを制御することができます。

## Connecting

OUTコネクタのいずれかに接続します。

### Raspberry Pi
![](/img/100_analog/connect/112_connect_with_rasppi.jpg)

## Parts Specification
| Document |
|:--|
| [OSI5LA5113A](http://akizukidenshi.com/catalog/g/gI-04311/) |

## Schematic
![](/img/100_analog/schematic/112_ir_led.png)

## Sample Code

A0コネクタに赤外線LED Brickを接続し、１秒間隔で赤外線LEDを発光させます。

```python
# coding: utf-8
#
# FaBo Brick Sample
#
# #112 IR LED Brick
#

import RPi.GPIO as GPIO
import time

IR_LED_PIN = 4

GPIO.setwarnings(False)
GPIO.setmode(GPIO.BCM)
GPIO.setup(IR_LED_PIN, GPIO.OUT)

if __name__ == '__main__':
    try:
        while True:
            GPIO.output(IR_LED_PIN, True)
            time.sleep(1.0)
            GPIO.output(IR_LED_PIN, False)
            time.sleep(1.0)

    except KeyboardInterrupt:
        GPIO.cleanup()

```

## 構成Parts
- 5mm 赤外線LED OSI5LA5113A

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/112_ir_led
