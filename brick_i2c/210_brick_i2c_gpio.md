# #210 GPIO I2C Brick

<center>![](/img/200_i2c/product/210.jpg)
<!--COLORME-->

## Overview
汎用I/O拡張チップを使用したBrickです。

I2Cで8個のLEDを制御できます。

## Connecting
I2Cコネクタへ接続します。

![](/img/200_i2c/connect/210_gpio_connect.jpg)

## PCAL6408 Datasheet
| Document |
| -- |
| [PCAL6408 Datasheet](http://www.nxp.com/documents/data_sheet/PCAL6408A.pdf) |

## Register
| Slave Address |
| -- |
| 0x20 |

## Schematic
![](/img/200_i2c/schematic/210_gpio.png)

## Library

- pipからインストール
```
pip install FaBoGPIO_PCAL6408
```
- [Library GitHub](https://github.com/FaBoPlatform/FaBoGPIO-PCAL6408-Python)
- [Library Document](http://fabo.io/doxygen/FaBoGPIO-PCAL6408-Python/)

## Sample Code

```python
# coding: utf-8
## @package FaBoGPIO_PCAL6408.py
#  This is a library for the FaBo GPIO I2C Brick.
#
#  http://fabo.io/210.html
#
#  Released under APACHE LICENSE, VERSION 2.0
#
#  http://www.apache.org/licenses/
#
#  FaBo <info@fabo.io>

import FaBoGPIO_PCAL6408
import time
import sys

pcal6408 = FaBoGPIO_PCAL6408.PCAL6408()

try:
    while True:
        for i in xrange(8):
            pcal6408.setDigital(1<<i, 1)
            time.sleep(1)

        pcal6408.setAllClear()
        time.sleep(1)

except KeyboardInterrupt:
    pcal6408.setAllClear()
    sys.exit()
```

## 構成Parts
- NXP PCAL6408

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/210_gpio
