# Arduino IDE Library Managerについて
## 参考サイト
[Library Manager FAQ](https://github.com/arduino/Arduino/wiki/Library-Manager-FAQ)

## ライブラリマネージャに載せてもらう方法
### トップディレクトリの構成
- library.properties
- examples/スケッチ例のinoファイル
- src/ライブラリソース
- ライセンス
- README

### Githubにソースをおく
1. [Arduino Style Guide for Writing Libraries](http://www.arduino.cc/en/Reference/APIStyleGuide)にのっとる
2. Publicで公開する
3. Release(tag)を作る

### ArduinoのGithubでIssueを出す
https://github.com/arduino/Arduino/issues
- ライブラリを置いたgithubリポジトリのURLを書いて、祈りながら待つ

## 覚え書き
### library_index.json
http://downloads.arduino.cc/libraries/library_index.json
- IDEからこのjsonを読み込んでいるようだ。
- 毎時間ごとに作られるようだ。
