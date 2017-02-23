# ちびファイ2 MZK-UE150N

## 固定IP

`/etc/dhcpcd.conf`

```
interface eth1
static ip_address=192.168.111.100/24
static routers=192.168.111.1
```

## ネットワークの再起動

```
$ /etc/init.d/network restart
```