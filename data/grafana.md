# Grafana

## Grafanaのインストール

```shell
echo "deb https://dl.bintray.com/fg2it/deb jessie main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
sudo apt update
sudo apt -y install grafana
sudo systemctl enable grafana-server
sudo update-rc.d grafana-server defaults
```

