# WifiSwitch

## debを取得

```shell
$ wget https://raw.githubusercontent.com/FaBoPlatform/RaspberryPIDocs/master/packages/fabo-wifiswitch_1.1.0_armhf.deb
```

## isc-dhcp-serverを無効化
```shell
sudo systemctl stop isc-dhcp-server
sudo systemctl disable isc-dhcp-server
```

## インストール

```shell
$ sudo apt-get install hostapd dnsmasq
$ sudo dpkg -i fabo-wifiswitch_1.1.0_armhf.deb
```

## パスを通す

```shell
$ sudo sh -c "echo 'export PATH=\$PATH:/opt/fabo/bin' >> /root/.bashrc"
```

## IP転送関連の設定

`/etc/sysctl.conf` の`net.ipv4.ip_forward=1`のコメントアウトを取り除く

```shell
net.ipv4.ip_forward=1
```

IP転送を有効にする

```shell
$ sudo sh -c "echo 1 > /proc/sys/net/ipv4/ip_forward"
```

IP転送が有効になっているかどうか確認する

```shell
$ /sbin/sysctl net.ipv4.ip_forward
net.ipv4.ip_forward=1
```

## dhcpcd関連

dhcpcdがwlan0を使わないようにする

```shell
$ sudo sh -c "echo 'denyinterfaces wlan0' >> /etc/dhcpcd.conf"
```

## WiFi接続先設定

`/etc/wpa_supplicant/wpa_supplicant.conf`を記述する

```shell
country=JP
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
	ssid="myssid"
	psk="password"
}

```

## Mode変更

Access Point Mode

```shell
$ sudo /opt/fabo/bin/wifi_switch --mode ap
```

DHCP mode

```shell
$ sudo /opt/fabo/bin/wifi_switch --mode dhcp
```

STATIC mode

Defualt Datewayは192.168.0.1、Defaultの自分のIPは192.168.0.65

```shell
$ sudo /opt/fabo/bin/wifi_switch --mode static
```

## FaBo WiFi Switch GPIO Daemonインストール
Fabo #605 Motor Shield Raspberry Pi上にあるAP/Slaveスイッチに反応してWifiSwitchを実行し、APモード/DHCPモードに切り替える機能。

N-RESETのジャンパピンを外してスイッチの操作もしくはOS起動を行うと、WifiSwitchの設定ファイル(/opt/fabo/conf/wifi_switch.conf)が初期化されます。

``` shell
sudo apt-get install -y python-pip python-rpi.gpio
sudo pip install futures
wget https://raw.githubusercontent.com/FaBoPlatform/RaspberryPIDocs/master/packages/fabo-wifiswitch-gpiod_1.0.1_armhf.deb
sudo dpkg -i fabo-wifiswitch-gpiod_1.0.1_armhf.deb

sudo systemctl enable wifi_switchd
sudo systemctl start wifi_switchd
```