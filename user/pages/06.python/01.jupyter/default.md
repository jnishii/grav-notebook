---
title: Jupyter Notebook
---

[Jupyter Notebook](https://jupyter.org/)は，PythonやR, Juliaなどのコードを書いたり実行したり，文章を書いたりするのに開発環境(Mathematicaのノートブックのようなもの)です。


anaconda/python をインストールしていれば，Jupyter Notebookももれなく使えます。
```
$ jupyter notebook
```

## Rを Jupyter Notebookで使えるようにする

**方法1:** Rに必要なパッケージをインストールする。

Jupyter Notebook の起動前にRコンソールで以下を実行しておく。
```R
$ R
> install.packages(c('repr', 'IRdisplay', 'evaluate', 'crayon', 'pbdZMQ', 'devtools', 'uuid', 'digest'))
> devtools::install_github('IRkernel/IRkernel')
> IRkernel::installspec()  
```

**方法2:** anaconda上にRをインストールする
```
$ conda create -n r -c r r-irkernel
```


## 起動オプション
起動オプションは`jupyter notebook ―help`で表示される。オプションを指定するための設定ファイルを作りたいときは，まず以下のコマンドで設定ファイルを生成する。
```
$ jupyter notebook --generate-config
```
これで，`~/.jupyter/jupyter_notebook_config.py`に設定ファイルができるので，必要に応じてこれをいじる。

## 拡張機能のインストール

セルを畳んだりできるように[拡張機能(Jupyter notebook extensions)](https://github.com/ipython-contrib/jupyter_contrib_nbextensions)のインストールをする。

```
$ conda install -c conda-forge jupyter_contrib_nbextensions
$ jupyter nbextension enable codefolding/main
```
<!-- $jupyter contrib nbextension install --symlink -->

## スライド対応にする

いろいろな方法がある。

### 方法1: そのまま

メニュー「View/Cell Toolbar/Slideshow」を選択すると，各セルをスライドにするとかしないとか選択できる。
最後に以下でスライドモードで表示を行う。

```
$ jupyter nbconvert <file name>.ipynb --to slides --post serve
```

### 方法2: RISE

[RISE](https://github.com/damianavila/RISE)を使う。
お手軽に，そこそこに便利に使えるお勧めの方法。
```
$ conda install -c damianavila82 rise
```
jupyter notebook のメニューにスライド表示のボタンが出て，それをクリックすれば直ちにプレゼンを出来る。	
プレゼン上でプログラムの実行もできる。
どのセルをスライドにするかは，方法1と同様にして選択できる。


### 方法3: nbpresent
[nbpresent](https://github.com/Anaconda-Platform/nbpresent)を使う。
スライド作成のいろいろな機能がある。
```
conda install -c conda-forge nbpresent
```

### 方法4: nbviewer
GitHubにJupyter notebookをおけば，[Jupyter Notebook Viewer](http://nbviewer.jupyter.org)で表示できる。
プレゼン形式にも出来る。


## Short Cut Key
- コマンドモードで”h”を押すと一覧表示される
- [キーバインディングを emacs styleにする](https://github.com/rmcgibbo/jupyter-emacskeys)
```
$ pip install jupyter-emacskeys
```

## Magic command
各セルに`%`や`%%`ではじまるコマンドを書くことで，いろいろな処理ができる。

### shell scriptの実行
```
In [1] %%bash
       echo $PATH
```

## その他
- LaTeXコマンド: $$で囲めばOK
