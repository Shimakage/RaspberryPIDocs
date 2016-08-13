# #217 Ambient Light I2C Brick

<center>![](/img/200_i2c/product/217.jpg)
<!--COLORME-->

## Overview
照度センサーを使ったBrickです。

I2Cで明るさを取得することができます。

## Connecting
I2Cコネクタへ接続します。

![](/img/200_i2c/connect/217_ambientlight_connect.jpg)

## ISL29034 Datasheet
| Document |
| -- |
| [ISL29034 Datasheet](http://www.intersil.com/content/dam/Intersil/documents/isl2/isl29034.pdf) |

## Register
| Slave Address |
| -- |
| 0x44 |

## Schematic
![](/img/200_i2c/schematic/217_ambientlight.png)

## Library

- pipからインストール
```
pip install FaBoAmbientLight_ISL29034
```
- [Library GitHub](https://github.com/FaBoPlatform/FaBoAmbientLight-ISL29034-Python)
- [Library Document](http://fabo.io/doxygen/FaBoAmbientLight-ISL29034-Python/)

## Sample Code

上記のRapberryPI Python Libraryをインストールしてからご使用ください。

```python
# coding: utf-8
## @package FaBoRTC_PCF2129
#  This is a library for the FaBo Ambient Light I2C Brick.
#
#  http://fabo.io/217.html
#
#  Released under APACHE LICENSE, VERSION 2.0
#
#  http://www.apache.org/licenses/
#
#  FaBo <info@fabo.io>

import FaBoAmbientLight_ISL29034
import time
import sys

light = FaBoAmbientLight_ISL29034.ISL29034()

try:
    while True:

        lux  = light.read()

        print "Lux = ", lux
        time.sleep(0.5)

except KeyboardInterrupt:
    sys.exit()
```

## 構成Parts
- Intersil ISL29034

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/217_ambientlight
