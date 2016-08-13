# #110 Tilt Brick

<center>![](/img/100_analog/product/110.jpg)
<!--COLORME-->

## Overview
傾斜センサーを使用したBrickです。

I/Oピンより傾斜センサーの状態をデジタル値(0〜1)取得することができます。

黒い部分の中に玉が入っていて傾くとデジタル値が変化します。

LED Brickを点灯/消灯させる際などに使用します。


## Connecting
GPIOコネクタのいずれかに接続します。

### IchigoJam
OUTコネクタのいずれかに接続します。

## Schematic
![](/img/100_analog/schematic/110_tilt.png)

## Sample Code

GPIO5コネクタに接続したTilt Brickの傾きによって、GPIO4コネクタに接続したLED Brickを点灯/消灯させています。

```python
#!/usr/bin/env python
# coding: utf-8

#
# FaBo Brick Sample
#
# #110 Tilt  Brick
#

import RPi.GPIO as GPIO

LED_PIN = 4
TILT_PIN = 5

GPIO.setwarnings(False)
GPIO.setmode(GPIO.BCM)
GPIO.setup(LED_PIN, GPIO.OUT)
GPIO.setup(TILT_PIN, GPIO.IN)

if __name__ == '__main__':
    try:
        while True:
            if(GPIO.input(TILT_PIN)):
                GPIO.output(LED_PIN, True)
            else:
                GPIO.output(LED_PIN, False)

    except KeyboardInterrupt:
        GPIO.cleanup()
```

## 構成Parts
- 傾斜(振動)スイッチ

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/110_tilt
