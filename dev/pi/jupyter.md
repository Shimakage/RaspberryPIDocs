# Jupyter

## PIPのインストール

```shell
$ sudo apt-get install python-dev
$ sudo wget https://bootstrap.pypa.io/get-pip.py
$ sudo python ./get-pip.py
```

* `sudo apt-get install python-pip`ではうまくpipがインストールできない。

## Jupyterのインストール

```shell
$ sudo pip install jupyter
```

30分ほど時間がかかります。

## 他の便利なライブラリのインストール

```shell
$ sudo pip install panda numpy
```

10分ほど時間がかかります。

```shell
$ sudo apt-get install python-matplotlib
```



## 固定Tokenの作成

```
$ jupyter notebook --generate-config
```

```
$ vi ~/.jupyter/jupyter_notebook_config.py
```

c.NotebookApp.tokenのコメントアウトを消し、任意のTokenを指定する。(今回はfaboと指定)

`~/.jupyter/jupyter_notebook_config.py`
```
## Token used for authenticating first-time connections to the server.
#
#  When no password is enabled, the default is to generate a new, random token.
#
#  Setting to an empty string disables authentication altogether, which is NOT
#  RECOMMENDED.
c.NotebookApp.token = 'fabo'
```

## Jupyterの自動起動

```
$ sudo vi /etc/rc.local
```

`/etc/rc.local`
```
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.
# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.

# Print the IP address
_IP=$(hostname -I) || true
if [ "$_IP" ]; then
  printf "My IP address is %s\n" "$_IP"
fi

set -- $_IP
jupyter notebook --ip=$1 --notebook-dir=/home/pi/ --config=/home/pi/.jupyter/jupyter_notebook_config.py &

exit 0
```

## Browserから起動

```
$ ifconfig -a
```

RaspberryPIのIPアドレスを確認し、自分のマシンのブラウザから

> http://192.168.x.x:8888/

8888番ポートに接続する。

