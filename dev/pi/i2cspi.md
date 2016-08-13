#I2CとSPIの有効化の設定方法

OUT/INシールドとBrickを使う際に、I2CとSPIを有効化する必要があります。

1. インターネットへ接続できる様に設定してください
 
* 最新版にアップデートします
```shell
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
sudo rpi-update
sudo reboot
```
* raspi-configでI2CおよびSPIの有効化をします
```shell
sudo raspi-config
 メニューから[8 Advanced Options]>[A6 SPI]および[A7 I2C]を選択して有効化します
```
* I2C動作確認用のパッケージをインストールします
```shell
sudo apt-get install i2c-tools
```
* モジュールが自動起動するように設定します
```shell
vi /etc/modules
```
最後の行に、次の行を追加します。
```
i2c-dev
```
* 再起動します
```shell
sudo reboot
```
* I2CのBrickが接続されている場合、次のコマンドでI2Cアドレスが表示されます
```shell
sudo i2cdetect -y 1
```
* サンプルで使用している、Pythonモジュールをインストールします
```shell
sudo apt-get install python-dev
sudo apt-get install python-smbus
sudo apt-get install python-rpi.gpio
git clone git://github.com/doceme/py-spidev
cd py-spidev
sudo python setup.py install
```

