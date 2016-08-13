# PIとWifiSpotでネットワーク構築

## USB Wifiモジュールの確認(Raspberry PI側)

    $ inconfig -a

wlan0が有効になっていれば、USB Wifi モジュールは認識されている

![](/img/dev/pi/pi101.png)

## WifiSpotの情報をメモする

| 項目 | 値 |
| -- | -- |
| ネットワーク名(SSID) | SSID1_AAAAA |
| パスワード | ######## |

## SSID, PASSの設定(Raspberry PI側)

iPhoneのティザリングスポットを確認する

    $ sudo iwlist wlan0 scan | grep ネットワーク

suでSuperUserに変わる。

    $ sudo su

pass_pharaseを追加する。    
    
    $ wpa_passphrase 'SSID' 'pass' >> /etc/wpa_supplicaion/wpa_supplicat.conf
 
'SSID'と'pass'には、WifiSpotのメモしたネットワーク名とパスワードをいれる。

また、id_strの項目を追加し、識別できるようにしておく。

`/etc/wpa_supplicant/wpa_supplicant.conf`

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
        ssid="akira"
        #psk="########""
        psk=75d54dab398dcef5bc7fe3be5803f50083c0de5a46955d2c54aa30844d3ba30e
        id_str="iPhone"
}
network={
        ssid="SSID_AAAAAA"
        #psk="########"
        psk=0716a436b7e265d026cf1a10a7eb26bab9608648ac1a76e7ef2b5bc45dab6ce5
        id_str="Wifi1"
}
```



