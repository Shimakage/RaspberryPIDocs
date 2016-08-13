# #114 UV Brick

<center>![](/img/100_analog/product/114.jpg)
<!--COLORME-->

## Overview
紫外線センサーを使用したBrickです。I/Oピンより、紫外線の強弱をアナログ値(0〜1023)で取得することができます。

## Connecting
### Arduino
アナログコネクタ(A0〜A5)のいずれかに接続します。

### IchigoJam
アナログ用コネクタ(IN2またはANA()で設定したコネクタ)のどれかに接続します。

## Support
|Arduino|IchigoJam|
|:--:|:--:|
|◯|◯|

## Schematic
![](/img/100_analog/schematic/114_uv.png)

## Sample Code
### for Arduino
A0コネクタに接続し、紫外線の強弱をアナログ値で出力します。

```c
//
// FaBo Brick Sample
//
// #114 UV Brick
//

#define uvPin A0

int uvValue = 0;

void setup() {
  pinMode(uvPin,INPUT);
  Serial.begin(9600);
}

void loop() {
  uvValue = analogRead(uvPin) ;
  Serial.println(uvValue) ;
  delay(10);
}
```
### for IchigoJam

```Basic
100 'UV_sample_program
110 CLS
120 LOCATE 10,8:PRINT "Digital =";IN(2)
130 LOCATE 10,9:PRINT "Analog  =";ANA(2);"  "
140 GOTO 120
```
紫外線に反応して数値が変化します。

## Parts
- GaAsPフォトダイオードG6262

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/114_uv
