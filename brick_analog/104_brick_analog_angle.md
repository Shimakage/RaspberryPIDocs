# #104 Angle Brick

<center>![](/img/100_analog/product/104.jpg)
<!--COLORME-->

## Overview
ボリューム抵抗を使ったBrickです。

I/Oピンからアナログ値を取得することができます。

LED Brickの明るさを調節する際などに使用します。

## Connecting

### Arduino
アナログコネクタ(A0〜A5)のいずれかに接続します。

![](/img/100_analog/connect/104_angle_connect.jpg)

### Raspberry PI
アナログコネクタ(A0〜A7)のいずれかに接続します。

### IchigoJam
アナログ用コネクタ(IN2またはANA()で設定したコネクタ)のどれかに接続します。


## Support
|Arduino|RaspberryPI|IchigoJam|
|:--:|:--:|:--:|
|◯|◯|◯|

##Schematic
![](/img/100_analog/schematic/104_angle.png)

## Sample Code
### for Arduino
A0コネクタにAngleを接続して、D3コネクタに接続したLED Brickの明るさ調節に使用しています。

```c
//
// FaBo Brick Sample
//
// #104 Angle Brick
//

#define anglePin A0 // Angleピン
#define ledPin 3    // LEDピン

int angleValue = 0;
int outputValue = 0;

void setup() {
  // Angleピンを入力用に設定
  pinMode(anglePin, INPUT);
  // LEDピンを出力用に設定
  pinMode(ledPin, OUTPUT);
}

void loop() {
  // Angleから値を取得(0〜1023)
  angleValue = analogRead(anglePin);
  // analogWrite用に取得した値を変換
  outputValue = map(angleValue, 0, 1023, 0, 255);
  // PWMによりLED点灯
  analogWrite(ledPin, outputValue);
}
```

### for Raspbery PI
A0コネクタにAngle Brickを接続して、D3コネクタに接続したLED Brickの明るさ調節に使用しています。
```python
#!/usr/bin/env python
# coding: utf-8

#
# FaBo Brick Sample
#
# #104 Angle Brick
#

import RPi.GPIO as GPIO
import spidev
import time
import sys

# A0コネクタにAngleを接続
ANGLEPIN = 0

# GPIO4コネクタにLEDを接続
LEDPIN = 4

# GPIOポートを設定
GPIO.setwarnings(False)
GPIO.setmode( GPIO.BCM )
GPIO.setup( LEDPIN, GPIO.OUT )

# PWM/100Hzに設定
LED = GPIO.PWM(LEDPIN, 100)

# 初期化
LED.start(0)
spi = spidev.SpiDev()
spi.open(0,0)

def readadc(channel):
	adc = spi.xfer2([1,(8+channel)<<4,0])
	data = ((adc[1]&3) << 8) + adc[2]
	return data

def arduino_map(x, in_min, in_max, out_min, out_max):
	return (x - in_min) * (out_max - out_min) // (in_max - in_min) + out_min

if __name__ == '__main__':
	try:
		while True:
			data = readadc(ANGLEPIN)
			print("adc : {:8} ".format(data))
			value = arduino_map(data, 0, 1023, 0, 100)
			LED.ChangeDutyCycle(value)
			time.sleep( 0.01 )
	except KeyboardInterrupt:
		LED.stop()
		GPIO.cleanup()
		spi.close()
		sys.exit(0)
```


### for IchigoJam
#####注意<br>アナログはIN2のみで数値取得可能です。
デジタルの場合はIN(2)、アナログの場合がANA(2)とします。

```
100 'ANGLE_sample_program
110 CLS
120 LOCATE 10,8:PRINT "Digital =";IN(2)
130 LOCATE 10,9:PRINT "Analog  =";ANA(2);"  "
140 GOTO 120
```

つまみを回すと数値が変化します。

### for NRF52

```c

#include <stdbool.h>
#include <stdint.h>
#include <stdio.h>
#include "nrf.h"
#include "nrf_drv_saadc.h"
#include "app_uart.h"
#include "app_error.h"
#include "nrf_delay.h"
#include "app_util_platform.h"
#include <string.h>

#define UART_TX_BUF_SIZE 256 /**< UART TX buffer size. */
#define UART_RX_BUF_SIZE 1   /**< UART RX buffer size. */

#define FABO_RX 22
#define FABO_TX 23
#define FABO_CTS 0
#define FABO_RTS 0

#define FABO_A0 3

static nrf_saadc_value_t       m_buffer_pool[1];
static uint32_t                m_adc_evt_counter;

/**
 * @brief UART events handler.
 */
void uart_events_handler(app_uart_evt_t * p_event)
{
}

/**
 * @brief UART initialization.
 */
void uart_config(void)
{
    uint32_t                     err_code;
    const app_uart_comm_params_t comm_params =
    {
        FABO_RX,
        FABO_TX,
        FABO_RTS,
        FABO_CTS,
        APP_UART_FLOW_CONTROL_DISABLED,
        false,
        UART_BAUDRATE_BAUDRATE_Baud38400
    };

    APP_UART_FIFO_INIT(&comm_params,
                       UART_RX_BUF_SIZE,
                       UART_TX_BUF_SIZE,
                       uart_events_handler,
                       APP_IRQ_PRIORITY_LOW,
                       err_code);

    APP_ERROR_CHECK(err_code);
}

void saadc_callback(nrf_drv_saadc_evt_t const * p_event)
{
    if (p_event->type == NRF_DRV_SAADC_EVT_DONE)
    {
        ret_code_t err_code;

        err_code = nrf_drv_saadc_buffer_convert(p_event->data.done.p_buffer, 1);
        APP_ERROR_CHECK(err_code);

        printf("ADC event number: %d\r\n",(int)m_adc_evt_counter);
		printf("%d\r\n", p_event->data.done.p_buffer[0]);
        m_adc_evt_counter++;
    }
}

void saadc_init(void)
{
    ret_code_t err_code;
    nrf_saadc_channel_config_t channel_config =
            NRF_DRV_SAADC_DEFAULT_CHANNEL_CONFIG_SE(FABO_A0);
    err_code = nrf_drv_saadc_init(NULL, saadc_callback);
    APP_ERROR_CHECK(err_code);

    err_code = nrf_drv_saadc_channel_init(0, &channel_config);
    APP_ERROR_CHECK(err_code);

    err_code = nrf_drv_saadc_buffer_convert(m_buffer_pool,1);
    APP_ERROR_CHECK(err_code);
}

/**
 * @brief Function for main application entry.
 */
int main(void)
{
    uart_config();

    printf("\n\rSAADC HAL simple example.\r\n");

	saadc_init();

    while(true)
    {
	    ret_code_t err_code;
		err_code = nrf_drv_saadc_sample();
		APP_ERROR_CHECK(err_code);
		nrf_delay_ms(1000);
    }
}

```
## for Cylon.js

```js
var Cylon = require('cylon');

Cylon.robot({
        connections: {
                arduino: { adaptor: 'firmata', port: '/dev/tty.usbmodem1421' }
        },

        devices: {
                sensor: { driver: 'analog-sensor', pin: 0, lowerLimit: 0, upperLimit: 1023}
        },

        work: function(my) {

                var angleValue = 0;

                every((1).second(), function(){
                        angleValue = my.sensor.analogRead();
                        console.log('angle value =>', angleValue);
                });


                my.sensor.on('lowerLimit', function(val){
                        console.log('Lower angle value =>', val);
                });

                my.sensor.on('upperLimit', function(val){
                        console.log('upper angle value =>', val);
                });
        }
}).start();
```

### for Edison
A0コネクタにAngleを接続して、D3コネクタに接続したLED Brickの明るさ調節に使用しています。

```js
//
// FaBo Brick Sample
//
// #104 Angle Brick
//

//library
var m = require('mraa');

//pin setup
var angle_pin = new m.Aio(0); //Angle pin A0
var led_pin   = new m.Pwm(3); //LED pin D3

var value = 0.0;

led_pin.enable(true);
led_pin.period_us(2000);

//call loop function
loop();

function loop()
{

  var get_value = angle_pin.read()
  value = get_value / 1024;

  led_pin.write(value);

  //100 milliseconds
  setTimeout(loop,100);
}

```


## Parts
- ボリューム抵抗器A 10k

## GitHub
- https://github.com/FaBoPlatform/FaBo/tree/master/104_angle
