# #205 Proximity I2C Brick

<center>![](/img/200_i2c/product/205.jpg)
<!--COLORME-->

## Overview
近接センサーを使ったBrickです。

I2Cでデータを取得できます。

## Connecting
I2Cコネクタへ接続します。

![](/img/200_i2c/connect/205_proximity_connect.jpg)


## VCNL4010 Datasheet
| Document |
|:--|
| [VCNL4010 Datasheet](https://www.adafruit.com/images/product-files/466/vcnl4010.pdf) |

## Register
| I2C Slave Address |
|:-- |
| 0x13 |

## Schematic
![](/img/200_i2c/schematic/205_proximity.png)

## Library

- pipからインストール
```
pip install FaBoProximity_VCNL4010
```
- [Library GitHub](https://github.com/FaBoPlatform/FaBoProximity-VCNL4010-Python)
- [Library Document](http://fabo.io/doxygen/FaBoProximity-VCNL4010-Python/)

## Sample Code

上記のRapberryPI Python Libraryをインストールしてからご使用ください。

```python
# coding: utf-8
## @package FaBoProximity_VCNL4010
#  This is a library for the FaBo Proximity I2C Brick.
#
#  http://fabo.io/205.html
#
#  Released under APACHE LICENSE, VERSION 2.0
#
#  http://www.apache.org/licenses/
#
#  FaBo <info@fabo.io>

import FaBoProximity_VCNL4010
import time
import sys

vcnl4010 = FaBoProximity_VCNL4010.VCNL4010()

try:
    while True:
        prox = vcnl4010.readProx()
        ambi = vcnl4010.readAmbi()

        print "Prox = ", prox,
        print "Ambi = ", ambi

        time.sleep(1)

except KeyboardInterrupt:
    sys.exit()
```

## Parts
- Vishay VCNL4010

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/205_proximity
