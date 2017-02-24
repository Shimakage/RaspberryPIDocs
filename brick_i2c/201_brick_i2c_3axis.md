# #201 3Axis I2C Brick

<center>![](/img/200_i2c/product/201.jpg)
<!--COLORME-->

## Overview
3軸加速度センサーを使用したBrickです。

I2Cで3軸の加速度データを取得することがきます。

## センサー取得データについて
このBrickでは下の図の3軸の値を取得します。

![](/img/200_i2c/docs/201_3axis_docs_001.jpg)

それぞれ矢印の方向に力がかかるとプラス、逆方向ではマイナスとなります。

なお、このBrickを水平に置いた場合、重力がZ軸にかかっている状態となるので、X軸、Y軸が0に近く、Z軸のみ高い値となります。

## Connecting
4Pinケーブルで、OUT/INシールドのI2Cコネクタへ接続します。

※4Pinケーブル、各種OUT/INシールド、各種ケースは別売です。

![](/img/200_i2c/connect/201_3axis_connect.jpg)

写真はArduinoでの接続例です。

## ADXL345 Datasheet
| Document |
|:--|
| [ADXL345 Datasheet](http://www.analog.com/media/en/technical-documentation/data-sheets/ADXL345.pdf) |

## Register
| I2C Slave Address |
|:-- |
| 0x53 |

## Schematic
![](/img/200_i2c/schematic/201_3axis.png)

## Library

- pipからインストール
```
sudo pip install FaBo3Axis_ADXL345
```
- [Library GitHub](https://github.com/FaBoPlatform/FaBo3Axis-ADXL345-Python)
- [Library Document](http://fabo.io/doxygen/FaBo3Axis-ADXL345-Python/)

## Sample Code

I2Cコネクタに接続した3Axis I2C Brickより３軸の加速度情報を取得し、コンソールに出力します。

```python
# coding: utf-8

import smbus
import time


ADDRESS = 0x53
CHANNEL = 1

DATA_FORMAT = 0x31
POWER_CTL = 0x2d
DATA_XYZ = 0x32

bus = smbus.SMBus(CHANNEL)


class ADXL345:
	def __init__(self, address):
		self.address = address
		bus.write_byte_data(self.address, DATA_FORMAT, 0x00)
		bus.write_byte_data(self.address, POWER_CTL, 0x08)

	def read(self):
		data = bus.read_i2c_block_data(self.address, DATA_XYZ, 6)

		x = data[0] | (data[1] << 8)
		if(x & (1 << 16 - 1)):
			x = x - (1<<16)

		y = data[2] | (data[3] << 8)
		if(y & (1 << 16 - 1)):
			y = y - (1<<16)

		z = data[4] | (data[5] << 8)
		if(z & (1 << 16 - 1)):
			z = z - (1<<16)

		return {"x": x, "y": y, "z": z}


if __name__ == "__main__":
	adxl345 = ADXL345(ADDRESS)

	while True:
		axes = adxl345.read()
		print " x = " , ( axes['x'] )
		print " y = " , ( axes['y'] )
		print " z = " , ( axes['z'] )
		print
		time.sleep(1)

```

## 構成Parts
- Analog Devices ADXL345

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/201_3axis
