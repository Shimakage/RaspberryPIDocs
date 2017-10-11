# Raspberry Pi3用 Fabo WiFi Switch GPIO Daemon パッケージ

Fabo基板付属のWiFi切り替えスイッチによる WiFi AP / WiFi DHCP Client 切り替えツール


## fabo-wifiswitch-gpiod - v1.0

* Fabo WiFi Switch GPIO Daemon: Fabo基板付属のWiFi切り替えスイッチによる WiFi AP / WiFi DHCP Client 切り替えツール
  * https://github.com/FaBoPlatform/RaspberryPIDocs/package/fabo-wifiswitch-gpiod_1.0_armhf.deb
  * 必要なパッケージ:
    * [fabo-wifiswitch v1.0.1](./fabo-wifiswitch.md) 以上
    * apt-get install python-pip python-rpi.gpio
    * pip install futures
  * インストール：
    * dpkg -i fabo-wifiswitch-gpiod_1.0_armhf.deb


###### 機能説明
ジャンパピン[N-REST]が接続されている時、
  * /opt/fabo/bin/wifi_switch --print-config > /opt/fabo/conf/wifi_switch.conf

が実行される。confファイルを変更している場合はジャンパピンを外しておくこと。

MODEスイッチが[AP]の時、
  * /opt/fabo/bin/wifi_switch --mode ap

が実行される。

MODEスイッチが[Slave]の時、
  * /opt/fabo/bin/wifi_switch --mode dhcp

が実行される。


###### インストール手順
```
########################################
# FaBo WiFi Switch GPIO Daemonインストール
########################################
sudo apt-get install python-pip python-rpi.gpio
sudo pip install futures
sudo curl -o https://github.com/FaBoPlatform/RaspberryPIDocs/package/fabo-wifiswitch-gpiod_1.0_armhf.deb | dpkg -i
sudo systemctl enable wifi_switchd
sudo systemctl start wifi_switchd


########################################
# アンインストール
########################################
apt-get remove fabo-wifiswitch-gpiod


```