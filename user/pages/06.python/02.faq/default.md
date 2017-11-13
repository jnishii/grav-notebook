---
title: 困った時
---

## インストール関連のトラブル

### Q. pyenv で Anacondaをダウンロードできない

```
$ pyenv install anaconda3-4.4.0
Downloading Anaconda3-4.4.0-Linux-x86_64.sh...
-> https://repo.continuum.io/archive/Anaconda3-4.4.0-Linux-x86_64.sh
error: failed to download Anaconda3-4.4.0-Linux-x86_64.sh

BUILD FAILED (Vine 6.5 using python-build 1.1.3-27-g8297de9)
```
学外にアクセスするためにproxy設定は，以下のように設定済み。

```
$ cat ~/.bash_profile
...
export http_proxy="http://proxy.hoge..."
export https_proxy="https://proxy.hoge..."
export ftp_proxy="ftp://proxy.hoge..."
```
pyenvがダウンロードに使うcurlを使って，直接ダウンロードすると
```
$ curl https://repo.continuum.io/archive/Anaconda3-4.4.0-Linux-x86_64.sh
curl: (7) Unsupported proxy scheme for 'https://proxy.hoge....
```
とでる。ちなみに，curlのバージョンは
```
$ curl -V
curl 7.51.0 ...
```
proxy が　httpsの場合，curlのバージョンは7.55以上でないとダメらしい。
curlをupgradeすればよいが，とりあえずhttpsを偽造したら動いた。
```
$ https_proxy=http://proxy.cc.yamaguchi-u.ac.jp:8080 pyenv install anaconda3-4.4.0
Downloading Anaconda3-4.4.0-Linux-x86_64.sh...
```

ちなみに，curl単体で使う時のproxy設定は`~/.curlrc`に以下のように書けば良い。
```
$ cat ~/.curlrc
proxy="http://proxy.hogeport"
```
しかし，pyenvがcurlを呼び出す時にはhttpsのほうを押し付けられて，上記のような困ったことになるみたい。

## openCV + Python関連のトラブル

### Q. openCVを使って画像表示をしたあと，'cv2.destroyAllWindows()'を実行してもウィンドウが閉じない

'cv2.destroyAllWindows()'の直後に'cv2.keyWait(1)'を実行してみてください。
