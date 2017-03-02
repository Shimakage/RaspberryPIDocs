# #106 Touch Brick
<center>![](/img/100_analog/product/106.jpg)
<!--COLORME-->

## Overview
感圧センサーを使用したタッチセンサーBrickです。

I/Oピンより、感圧部分に加えられた力の大きさの変化をアナログ値で取得することができます。

## Connecting
アナログコネクタ(A0〜A7)のいずれかに接続します。

### Arduino
![](/img/100_analog/connect/106_touch_connect.jpg)
### Raspberr Pi
![](/img/100_analog/connect/104_connect_with_rasppi.jpg)


## Datasheet
| Document |
|:--|
| [Datasheet](http://interlinkelectronics.com/datasheets/Datasheet_FSR.pdf) |

## Schematic
![](/img/100_analog/schematic/106_touch.png)

## Sample Code

A0コネクタにTouchを接続して、GPIO4コネクタに接続したLED Brickの明るさ調節に使用しています。
```python
import RPi.GPIO as GPIO

LEDPIN = 4
BUTTONPIN = 5

GPIO.setwarnings(False)
GPIO.setmode( GPIO.BCM )
GPIO.setup( LEDPIN, GPIO.OUT )
GPIO.setup( BUTTONPIN, GPIO.IN )

if __name__ == '__main__':
    try:
        while True:
            # ボタン押下判定
            if( GPIO.input( BUTTONPIN ) ):
                # LED点灯
                GPIO.output( LEDPIN, True )
            else:
                # LED消灯
                GPIO.output( LEDPIN, False )
    except KeyboardInterrupt:
        GPIO.cleanup()
        sys.exit(0)
```

## 構成Parts
- 感圧センサー

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/106_touch
