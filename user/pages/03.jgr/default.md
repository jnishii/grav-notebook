---
title: グラフィックライブラリ
---


## JGRとは
JGR はX Window上でグラフを描くための C++ クラスライブラリです。

## JGRの特徴

- 複数のウインドウを管理でき, また, １つのウインドウ内に複数のグラフを管理できます。プログラムの実行中に随時計算結果をどんどんプロットしていきたい時に便利です。

- 点や線、四角、円などなども描けます。

- 通常、グラフを描く時にはX軸の座標の単位系（例えば[m],[s])における値とグラフィック画面上での値（ドット）が違いますが、このグラフィック・ツールではX、Y軸の長さ、実際の値（m,sなど）と画面上での値（ドット）の関係などをはじめに与えておくことによって、あとは自動的に実際の値から座標変換を行なって点や線を描いてくれます。

- Xlib の関数を使ってますが, Xlib を意識せずに使用できます。

- EPSファイルもはいてくれます。(バグありそうですが)

- メニューもつくれます。重いですが...^^;;

- Vine Linux, Mac OS Xで動作確認しています。

## ライセンス
PS関連ルーチン(jgrPS.cc)は XeasyGraphicsライブラリに含まれるXEGpsを改編して使っているため、XeasyGraphic- Copyright に準じます、それ以外はLGPLとします。

## ダウンロード
- GitHubからどうぞ: `https://github.com/jnishii/jgr`

<!-- - Source : [jgr-3.8.tar.gz](http://bcl.sci.yamaguchi-u.ac.jp/~jun/download/jgr/jgr-3.8.tar.gz) -->


## サンプルプログラム
-  [tiny.cc](http://bcl.sci.yamaguchi-u.ac.jp/~jun/download/jgr/samples/tiny.cc)
-  [sin.cc](http://bcl.sci.yamaguchi-u.ac.jp/~jun/download/jgr/samples/sin.cc)
-  [sample.cc](http://bcl.sci.yamaguchi-u.ac.jp/~jun/download/jgr/samples/sample.cc)
-  [mouse.cc](http://bcl.sci.yamaguchi-u.ac.jp/~jun/download/jgr/samples/mouse.cc)

ソースやrpmパッケージにはもういくつかのサンプルが入ってます。

## オンラインマニュアル
-  [jgr.html](http://bcl.sci.yamaguchi-u.ac.jp/~jun/download/jgr/html/)
-  [jgr.pdf](http://bcl.sci.yamaguchi-u.ac.jp/~jun/download/jgr/jgr.pdf)

## 謝辞
PS処理ルーチンは XeasyGraphic (Ver.1.01,荻原剛志氏作)
を改編して使っております。感謝いたします。
