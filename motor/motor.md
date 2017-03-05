# #601 Motor Shield for Arduino

RaspberryPIの奨励電圧は、5 V, 2.5 Aと定義されている。
PowerCore 20100は、最大電圧が4.8A。

|パーツ|電圧|
|:--|:--|
|DRV8830 |1A |
|RaspberryPI3|2.5A|

## DRV8830

* 動作電圧 2.75V-6.8V
* 最大駆動電流: 1A

## RaspberryPI3

* 動作電圧: 5V
* 奨励電流: 2.5A

## FA-130RA-18100

Noload(無負荷電流)
* 動作電圧: 3V
* 動作電流: 0.15A

Stall(停動電流)
* 動作電圧: 3V
* 動作電流: 2.1A

## ギアボックス

タミヤ ダブルギヤボックス （左右独立4速タイプ) 70168

|ギア比|回転数|回転トルク|
|:--|:--|
|12.7:1|1039rpm|94 gf・cm|
|38.2:1|345rpm|278 gf・cm|
|114.7:1|115rpm|809 gf・cm|
|344.2:1|38rpm|2276 gf・com|

## I2Cプログラム

```python
import smbus
import time
bus = smbus.SMBus(1)
forward_val = 0x02

def forward():
    speed = getspeed()
    cmd = 0x01
    sval = cmd | ((speed+6)<<2)
    stop()
    bus.write_i2c_block_data(0x64,0x00,[sval])

def back():
    speed = getspeed()
    cmd = 0x02
    sval = cmd | ((speed+6)<<2)
    stop()
    bus.write_i2c_block_data(0x64,0x00,[sval])

def stop():
    speed = getspeed()
    cmd = 0x00
    sval = cmd | ((speed+6)<<2)
    bus.write_i2c_block_data(0x64,0x00,[sval])

def brake():
    speed = getspeed()
    cmd = 0x03
    sval = cmd | ((speed+6)<<2)
    stop()
    bus.write_i2c_block_data(0x64,0x00,[sval])

def getspeed():
    rd = bus.read_word_data(0x64,[0x00])
    #スピードを0~59の範囲で取り出す
    speed = (rd >> 2)-6
    return speed

def setspeed(speed):
    rd = bus.read_word_data(0x64,[0x00])
    cmd = (rd & 0x03)
    sval = cmd | ((speed+6)<<2)
    bus.write_i2c_block_data(0x64,0x00,[sval])

setspeed(0)
forward()

for i in range(0x00,0x5A,1):
    setspeed(i)
    time.sleep(0.1)
        
for i in range(0x59,-0x01,-1):
    setspeed(i)
    time.sleep(0.1)

stop()
brake()

bus.close()
```

