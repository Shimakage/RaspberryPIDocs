# ネットワーク設定(Pixel)

## Wireless & Wired Networkの設定

Wireless & Wired Networkの設定を選択

![](/img/dev/pi/wifi01.png)

![](/img/dev/pi/wifi02.png)

USBWifiを使用している場合WLAN0を指定。DNSのアドレスは、GoogleのPublic DNSである`8.8.8.8`を設定。

![](/img/dev/pi/wifi03.png)

## Wifiへの接続

![](/img/dev/pi/wifi04.png)

![](/img/dev/pi/wifi05.png)

![](/img/dev/pi/wifi06.png)


## Gatewayの設定

```
$ ifconfig -a 
```

![](/img/dev/pi/wifi07.png)

```
$ sudo route add default gw 192.169.x.1
```

xはifconfig wlan0で表示された値をいれる。



