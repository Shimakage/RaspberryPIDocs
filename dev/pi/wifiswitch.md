# WifiSwitch

## debを取得

```shell
$ wget -O fabo-wifiswitch_1.0_armhf.deb https://github.com/FaBoPlatform/RaspberryPIDocs/blob/master/packages/fabo-wifiswitch_1.0_armhf.deb?raw=true 
```

## インストール

```shell
$ sudo apt-get install hostapd dnsmasq
$ sudo dpkg -i fabo-wifiswitch_1.0_armhf.deb
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

`/etc/wpa_supplicant/wpa_supplicant.conf`のcountryをJPにする

```shell
country=JP
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