# #204 Barometer I2C Brick

<center>![](/img/200_i2c/product/204.jpg)
<!--COLORME-->

## Overview
大気圧センサを使用したBrickです。

I2Cでデータを取得できます。

## Connecting
I2Cコネクタへ接続します。

![](/img/200_i2c/connect/204_barometer_connect.jpg)

## MPL115A2 Datasheet
| Document |
| -- |
| [MPL115A2 Datasheet](http://cache.freescale.com/files/sensors/doc/data_sheet/MPL115A2.pdf) |

## Register
| Slave Address |
| -- |
| 0x60 |

## Schematic
![](/img/200_i2c/schematic/204_barometer.png)

## Library

- pipからインストール
```
pip install FaBoBarometer_MPL115
```
- [Library GitHub](https://github.com/FaBoPlatform/FaBoBarometer-MPL115-Python)
- [Library Document](http://fabo.io/doxygen/FaBoBarometer-MPL115-Python/)

## Sample Code
I2Cコネクタに接続したBarometer Brickより、気圧、温度、標高212mの気圧を取得し、シリアルモニタに出力します。

```c
# coding: utf-8
## @package FaBoBarometer_MPL115
#  This is a library for the FaBo Barometer I2C Brick.
#
#  http://fabo.io/204.html
#
#  Released under APACHE LICENSE, VERSION 2.0
#
#  http://www.apache.org/licenses/
#
#  FaBo <info@fabo.io>


import FaBoBarometer_MPL115
import time
import sys

mpl115 = FaBoBarometer_MPL115.MPL115()

try:
    while True:
        data  = mpl115.readData()
        a_hpa = mpl115.readHpa(212.0)

        print "hpa  = ", data['hpa']
        print "temp = ", data['temp']
        print "hpa_aizu = ", a_hpa
        print
        time.sleep(1)

except KeyboardInterrupt:
    sys.exit()
```


## Parts
- Freescale(NXP) MPL115A2

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/204_barometer
