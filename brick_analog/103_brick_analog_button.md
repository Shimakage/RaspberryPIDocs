# #103 Button Brick

<center>![](/img/100_analog/product/103.jpg)
<!--COLORME-->

## Overview
ボタンを使ったBrickです。I/OピンよりボタンのON/OFFの状態を取得することができます。

※ボタンカバー部分の色はランダムで送付するため色のご指定はできません。あらかじめご了承ください。

## Connecting

GPIOコネクタのいずれかに接続します。

## Schematic
![](/img/100_analog/schematic/103_button.png)

## Sample Code

GPIO5コネクタに接続したButton Brickの入力により、GPIO4コネクタに接続したLED Brick の点灯/消灯を制御しています。

```python
# coding: utf-8
#
# FaBo Brick Sample
#
# brick_analog_button
#

import RPi.GPIO as GPIO

LEDPIN = 4
BUTTONPIN = 5

GPIO.setwarnings(False)
GPIO.setmode( GPIO.BCM )
GPIO.setup( LEDPIN, GPIO.OUT )
GPIO.setup( BUTTONPIN, GPIO.IN )

while True:
    # ボタン押下判定
	if( GPIO.input( BUTTONPIN ) ):
	    # LED点灯
        GPIO.output( LEDPIN, True )
	else:
	    # LED消灯
		GPIO.output( LEDPIN, False )
```

## 構成Parts
- 12mm角タクトスイッチ

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/103_button
