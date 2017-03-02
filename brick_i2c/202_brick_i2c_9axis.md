# #202 9Axis I2C Brick

<center>![](/img/200_i2c/product/202.jpg)
<!--COLORME-->

## Overview
1チップで3軸加速度、3軸ジャイロ、3軸コンパスを取得できるセンサを使用したBrickです。

I2Cでデータを取得できます。

## Connecting
I2Cコネクタへ接続します。

### Arduino
![](/img/200_i2c/connect/202_9axis_connect.jpg)
### Raspberry Pi
![](/img/200_i2c/connect/202_connect_with_rasppi.jpg)

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
$ sudo pip install FaBo9Axis_MPU9250
```
- [Library GitHub](https://github.com/FaBoPlatform/FaBo9AXIS-MPU9250-Python)
- [Library Document](http://fabo.io/doxygen/FaBo9AXIS-MPU9250-Python/)

## Sample Code


### for Raspberry Pi
I2Cコネクタに接続した9Axis I2C Brickより３軸加速度、３軸ジャイロ、３軸コンパス情報を取得し、コンソールに出力します。

```python
# coding: utf-8
import FaBo9Axis_MPU9250
import time
import sys

mpu9250 = FaBo9Axis_MPU9250.MPU9250()

try:
    while True:
        a = mpu9250.readAccel()
        g = mpu9250.readGyro()
        m = mpu9250.readMagnet() 
        value = (a['x'],  a['y'], a['z'], g['x'], g['y'], g['z'],m['x'],m['y'],m['z'])
        sys.stdout.write("\rax=%f, ay=%f, az=%f, gx=%f,gy=%f,gz=%f,mx=%f,my=%f,mz=%f" % value)
        sys.stdout.flush()
    
        time.sleep(0.3)

except KeyboardInterrupt:
    sys.exit()
```

import smbusに失敗する場合はraspi-configでI2CおよびSPIを有効化してください。

```shell
sudo raspi-config
```

メニューから[7 Advanced Options]>[P5 I2C]を選択して有効化します

## 構成Parts
- InvenSense MPU-9250

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/202_9axis
