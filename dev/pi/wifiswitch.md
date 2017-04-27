# WifiSwitch

## debを取得

```shell
$ curl -o https://github.com/FaBoPlatform/RaspberryPIDocs/blob/master/packages/fabo-wifiswitch_1.0_armhf.deb
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

`/etc/sysctl.conf` の`net.ipv4.ip_forward=1`を確認

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