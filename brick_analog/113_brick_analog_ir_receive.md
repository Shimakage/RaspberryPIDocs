# #113 IR Receiver Brick

<center>![](/img/100_analog/product/113.jpg)
<!--COLORME-->

## Overview
フォトトランジスタを使った赤外線受信Brickです。

I/Oピンから赤外線受信のON/OFFを取得することができます。

## Connecting
GPIOコネクタに接続します。

## Support
|Arduino|RaspberryPI|IchigoJam|
|:--:|:--:|:--:|
|◯|◯|◯|

## Parts Specification
| Document |
|:--|
| [L-51ROPT1D1](http://akizukidenshi.com/catalog/g/gI-04211/) |
| [2SC1815L-Y](http://akizukidenshi.com/catalog/g/gI-06475/) |

## Schematic
![](/img/100_analog/schematic/113_ir_receive.png)

## Sample Code

GPIO5コネクタに赤外線受信Brick、GPIO4コネクタにLED Brickを接続し、赤外線を受信したらLEDを発光させます。

```python
# coding: utf-8
#
# FaBo Brick Sample
#
# #113 IR Receiver Brick
#

import RPi.GPIO as GPIO

LED_PIN = 4
IR_RECEIVER_PIN = 5

GPIO.setwarnings(False)
GPIO.setmode(GPIO.BCM)
GPIO.setup(LED_PIN, GPIO.OUT)
GPIO.setup(IR_RECEIVER_PIN, GPIO.IN)

if __name__ == '__main__':
    try:
        while True:
            if(GPIO.input(IR_RECEIVER_PIN)):
                GPIO.output(LED_PIN, True)
            else:
                GPIO.output(LED_PIN, False)

    except KeyboardInterrupt:
        GPIO.cleanup()

```

## 構成Parts
- 赤外線フォトトランジスタ

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/113_ir_receive
