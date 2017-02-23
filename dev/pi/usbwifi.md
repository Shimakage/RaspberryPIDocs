# PIとWifiSpotでネットワーク構築

## USB Wifiモジュールの確認(Raspberry PI側)

```shell
$ ifconfig -a
```

wlan0が有効になっていれば、USB Wifi モジュールは認識されている

![](/img/dev/pi/pi101.png)

## WifiSpotの情報をメモする

| 項目 | 値 |
| -- | -- |
| ネットワーク名(SSID) | SSID1_AAAAA |
| パスワード | ######## |

## SSID, PASSの設定(Raspberry PI側)

iPhoneのティザリングスポットを確認する

```shell
$ sudo iwlist wlan0 scan | grep ネットワーク
```

例) 

```shell
$ sudo iwlist wlan0 scan | grep fabkura
ESSID:"fabkura-gclue"
ESSID:"fabkura-youkoso"
```

pass_pharaseを追加する。    

```shell    
$ sudo wpa_passphrase 'SSID' 'pass' >> /etc/wpa_supplicant/wpa_supplicant.conf
```

'SSID'と'pass'には、WifiSpotのメモしたネットワーク名とパスワードをいれる。

また、id_strの項目を追加し、識別できるようにしておく。

`/etc/wpa_supplicant/wpa_supplicant.conf`
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
        ssid="akira"
        #psk="########""
        psk=****************************************************************
        id_str="iPhone"
}
network={
        ssid="SSID_AAAAAA"
        #psk="########"
        psk=****************************************************************
        id_str="Wifi1"
}
```

ネットワーク・インターフェースを再起動。

```shell
$ sudo ifdown wlan0
$ sudo ifup wlan0
```

```
$ sudo ifconfig wlan0
```

## Default Gatewayの設定

```
$ sudo route add default gw 192.169.x.1
```

xはifconfig -aで表示された値をいれる。

