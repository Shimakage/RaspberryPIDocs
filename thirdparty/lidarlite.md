# Lidar Lite v3をRaspberry piのI2Cで制御する

## Overview
Lidar Lite V3の連携方法です。

Lidar Lite V3は小型かつ高性能なレーザーを利用した光学式測距モジュール。搭載可能面積が少ない小型ロボットや、ドローン、無人走行車などに最適です。

## Connecting
データシートより、以下の画像を参照してください。

![](https://i.imgur.com/JP9KDTY.png)

<dl>
  <dt>赤色</dt>
  <dd>5V入力(+)</dd>
  <dt>オレンジ</dt>
  <dd>今回は使用しません</dd>
  <dt>黄色</dt>
  <dd>今回は使用しません</dd>
  <dt>緑色</dt>
  <dd>I2CのSCLに接続</dd>
  <dt>青色</dt>
  <dd>I2CのSDAに接続</dd>
  <dt>黒色</dt>
  <dd>グラウンド(GND)</dd>
</dl>

\
Raspberry piに接続されているシールドのI2Cコネクタに接続する。なお、きちんと接続がされていればRaspberry pi上のTerminalにてコマンド

```
$ sudo i2cdetect -r -y 1
```

などを打てば。

![](https://i.imgur.com/sP3Ihpp.png)


このようにきちんと接続がされているということが分かります。
また、Lidar Lite v3のアドレスが 0x62 であるということもわかります。

## Support
|Arduino|RaspberryPI|
|:--:|:--:|
|◯|◯|

## LidarLiteV3 Datasheet
|Document|
|--|
|[LidarLiteV3](http://static.garmin.com/pumac/LIDAR_Lite_v3_Operation_Manual_and_Technical_Specifications.pdf)|

## Schematic

![](https://i.imgur.com/htZXPnR.png)

## I2Cの設定

あらかじめRaspberry pi側の設定としてI2Cを有効化しておきます。

（参考）

Terminal上で

```
$ sudo raspi-config
```
と入力、メニューの中から　`5 Interfacing Options` を選び、`P5 I2C` を選択　`Would you like the ARM I2C interface to be enable?`と問われるので `<Yes>`を選択すると、有効化が可能。

<br>

smbusをインストール

```
$ sudo apt-get install python-smbus
```
<br>

データシートより、距離を知るためにすることは以下の３つ
<br>

![](https://i.imgur.com/CWnTPhX.png)

<br>

1. 0x00のレジスタに0x04のレジスタの内容を書き込む
2. 0x01を読み出し、最下位bitが0になるまで繰り返す。
3. Highは0x0f、lowは0x10より、2byte読み込んで16bitの測定距離をcm単位で取得する。

## Sample

```python
# coding: utf-8
#lider lite v3

import smbus
import time

bus = smbus.SMBus(1)  #I2Cのバス番号
address = 0x62  #Lider Lite v3のアドレス

ACQ_COMMAND = 0x00
STATUS = 0x01
FULL_DELAY_HIGH = 0x0f
FULL_DELAY_LOW = 0x10

while True:
#0x00に0x04の内容を書き込む

     bus.write_block_data(address,ACQ_COMMAND,[0x04])

#0x01を読み込んで、最下位bitが0になるまで読み込む

     value = bus.read_byte_data(address, STATUS)
     while value & 0x01==1:
         value = bus.read_byte_data(address, STATUS)

#0x8fから2バイト読み込んで16bitの測定距離をcm単位で取得する

     high = bus.read_byte_data(address, FULL_DELAY_HIGH)
     low = bus.read_byte_data(address,FULL_DELAY_LOW)
     val = (high << 8) + low
     dist = val
     print "Dist = {0} cm , {1} m".format(dist,dist/100)

#cmを100倍してmに直す。

     time.sleep(1)

#time.sleep(1)で1秒ごとに距離を出力する。
```
