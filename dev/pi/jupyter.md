# Jupyter

## Jupyterのインストール

```
$ pip install jupyter
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
jupyter notebook --ip=$_IP --notebook-dir=/home/pi/ --config=/home/pi/.jupyter/
exit 0
```

