# #202 9Axis I2C Brick

<center>![](/img/200_i2c/product/202.jpg)
<!--COLORME-->

## Overview
1チップで3軸加速度、3軸ジャイロ、3軸コンパスを取得できるセンサを使用したBrickです。

I2Cでデータを取得できます。

## Connecting
I2Cコネクタへ接続します。

![](/img/200_i2c/connect/202_9axis_connect.jpg)

## MPU-9250 Datasheet
| Document |
| -- |
| [MPU-9250 Register Map](http://43zrtwysvxb2gf29r5o0athu.wpengine.netdna-cdn.com/wp-content/uploads/2015/02/MPU-9250-Register-Map.pdf) |
| [MPU-9250 Datasheet](http://43zrtwysvxb2gf29r5o0athu.wpengine.netdna-cdn.com/wp-content/uploads/2015/02/MPU-9250-Datasheet.pdf) |

## Register
MPU-9250は、三軸加速度、ジャイロ用とコンパス用の2つのI2C Slave Addressがあります。

### MPU-9250(三軸加速度、ジャイロ)
|Slave Address|
|--|--|
|0x68|

### AK8963(コンパス)
|Slave Address |
|--|--|
|0x0C|

## Schematic
![](/img/200_i2c/schematic/202_9axis.png)

## Library

- pipからインストール
```
pip install FaBo9Axis_MPU9250
```
- [Library GitHub](https://github.com/FaBoPlatform/FaBo9AXIS-MPU9250-Python)
- [Library Document](http://fabo.io/doxygen/FaBo9AXIS-MPU9250-Python/)

## Sample Code


### for Raspberry Pi
I2Cコネクタに接続した9Axis I2C Brickより３軸加速度、３軸ジャイロ、３軸コンパス情報を取得し、コンソールに出力します。

```python
# coding: utf-8
## @package faboMPU9250
#  This is a library for the FaBo 9AXIS I2C Brick.
#
#  http://fabo.io/202.html
#
#  Released under APACHE LICENSE, VERSION 2.0
#
#  http://www.apache.org/licenses/
#
#  FaBo <info@fabo.io>

import FaBo9Axis_MPU9250
import time
import sys

mpu9250 = FaBo9Axis_MPU9250.MPU9250()

try:
    while True:
        accel = mpu9250.readAccel()
        print " ax = " , ( accel['x'] )
        print " ay = " , ( accel['y'] )
        print " az = " , ( accel['z'] )

        gyro = mpu9250.readGyro()
        print " gx = " , ( gyro['x'] )
        print " gy = " , ( gyro['y'] )
        print " gz = " , ( gyro['z'] )

        mag = mpu9250.readMagnet()
        print " mx = " , ( mag['x'] )
        print " my = " , ( mag['y'] )
        print " mz = " , ( mag['z'] )
        print

        time.sleep(0.5)

except KeyboardInterrupt:
    sys.exit()
```

## 構成Parts
- InvenSense MPU-9250

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/202_9axis
