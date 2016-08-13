# #207 Temperature I2C Brick

<center>![](/img/200_i2c/product/207.jpg)
<!--COLORME-->

## Overview
温度センサを使用したBrickです。
I2Cでデータを取得できます。

計測できる範囲は−55度〜150度です。

## Connecting
I2Cコネクタへ接続します。

![](/img/200_i2c/connect/207_temperature_connect.jpg)

## ADT7410 Datasheet
| Document |
| -- |
| [ADT7410 Datasheet](http://www.analog.com/media/en/technical-documentation/data-sheets/ADT7410.pdf) |

## Register
| Slave Address |
| -- |
| 0x48 |

## Schematic
![](/img/200_i2c/schematic/207_temperature.png)

## Library

- pipからインストール
```
pip install FaBoTemperature_ADT7410
```
- [Library GitHub](https://github.com/FaBoPlatform/FaBoTemperature-ADT7410-Python)
- [Library Document](http://fabo.io/doxygen/FaBoTemperature-ADT7410-Python/)

## Sample Code

```python
# coding: utf-8
## @package faboTemperature.py
#  This is a library for the FaBo Temperature I2C Brick.
#
#  http://fabo.io/207.html
#
#  Released under APACHE LICENSE, VERSION 2.0
#
#  http://www.apache.org/licenses/
#
#  FaBo <info@fabo.io>

import FaBoTemperature_ADT7410
import time
import sys

adt7410 = FaBoTemperature_ADT7410.ADT7410()

try:
    while True:
        temp = adt7410.read()
        print "Temperature = ", temp

        time.sleep(0.5)

except KeyboardInterrupt:
    sys.exit()
```

## 構成Parts
- Analog Devices ADT7410

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/207_temperature
