---
title: Pythonのインストール
---

pyenv/anaconda を使って Mac上に python 環境を作る方法です。

- [pyenv](https://github.com/yyuu/pyenv): 複数のバージョンのpythonをインストールしたり，使うバージョンを切り替えたりするのに便利なツール。
- [Anaconda](https://www.continuum.io/why-anaconda): Pythonとデータ解析モジュールのディストリビューション

Macでhomebrewを使う人も，Linuxの人も以下の手順でOKのはずです。

## 準備: pyenv のインストール

1. `pyenv` 環境は，デフォルトでは`~/.pyenv/shims`に(つまり，自分のホーム内に)インストールされる。
Mac上にhomebrewでpyenvをインストールする場合，もし`/usr/local/`以下にインストールしたい時は，以下を`~/.bash_profile`に追加する。(**よくわかないとか，どっちでも良いヒトは設定不要**)
```
export PYENV_ROOT=/usr/local/var/pyenv
```
2. pyenvのインストール
Macでhomebrewを使う場合は以下で。
```
$ brew install pyenv
```
githubから直接ダウンロードするには以下の方法で(Linux等)。
```
$ cd ~/
$ git clone https://github.com/yyuu/pyenv.git .pyenv
```
3. パス設定: pyenvのコマンド群があるshimsにパスを通して，コマンド補完も出来る`~/.bash_profile`に設定を追加する。
```
$ echo "if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi" >> ~/.bash_profile
```
`~/.bash_profile`の読み込みをお忘れなく。
```
$ . ~/.bash_profile
```

## Anacondaをインストール

1. インストール可能なanacondaのバージョンチェック
```
$ pyenv install -l | grep anaconda
```
インストールできるバージョンがずらずら出てくるので，どれにするか選ぶ
(希望がなければとりあえず最新で)

2. anaconda のインストール
```
$ pyenv install anaconda3-5.0.0 # anaconda3-5.0.0 の python  は version 3.6.2
$ pyenv rehash
$ pyenv global anaconda3-5.0.0 # anaconda3-5.0.0 を利用するための設定
$ conda update conda                    # パッケージのアップデート
```

pythonのモジュールは一般に`pip`コマンドで管理するが，anaconda 環境の python モジュールは`conda`で管理する。(`conda` の使い方
は下の方にメモ書きあり。)

3. 無事インストールできたかを確認する
```
$ pyenv versions
	system
* anaconda3-5.0.0 (set by /usr/local/var/pyenv/version)
$ which python
  /Users/jun/.pyenv/shims/python
$ python --version
Python 3.6.2 :: Anaconda 5.0.0 (x86_64)
```

## pythonの複数バージョンを管理する方法

### 方法1(おすすめ): anaconda上に複数バージョンをインストールする

- 仮想環境の構築
anacondaでpython 3.6.1を使えるようにした環境で，さらにpython 2.7の仮想環境も構築したくなったら以下のようにインストールする。
```
$ conda create -n py27 python=2.7 anaconda
```
これで，python 2.7の仮想環境がインストールされて，その仮想環境名が`py27`になる。

- インストールされている仮想環境の確認は
```
$ conda info -e
py27		~/.pyenv/versions/anaconda-3-2.5.0/envs/py27
root	* ~/.pyenv/versions/anaconda-3-2.5.0
```
- 仮想環境の切り替え
```
$ source activate py27   <= py27にする
$ source deactivate      <= 仮想環境から出る
```
ただし，このactivateでターミナルが落ちるときには以下のように`activate`をフルパスで指定する。
```
$ source ${PYENV_ROOT}/versions/anaconda3-5.0.0/bin/activate
```
anacondaのバージョンは，インストールされているものに合わせて修正する。
面倒なときにはaliasを設定しておく。
```
$ echo 'alias activate=source ${PYENV_ROOT}/versions/anaconda3-2.5.0/bin/activate" >> ~/.bash_profile'
```
- 仮想環境の削除
```
$ conda remove -n py27 --all
```

### 方法2: 複数のバージョンのanacondaをインストールする

以下のようにanacondaの複数のバージョンをインストールして，必要に応じて切り替えることができる。
```
$ pyenv install anaconda3-2.5.0 # anaconda3-2.5.0 の python  は version 3.5.1
$ pyenv install anaconda3-5.0.0 # anaconda3-5.0.0 の python  は version 3.6.2
$ pyenv rehash
$ pyenv global anaconda3-5.0.0  # デフォルトのanaconda3のバージョンを選択
$ conda update conda            # パッケージのアップデート
$ pyenv versions                # 確認
	system
* anaconda3-5.0.0 (set by /usr/local/var/pyenv/version)
	anaconda3-2.5.0
$ python --version
Python 3.6.2 :: Anaconda 5.0.0 (x86_64)
```

## conda の使い方
`conda` は anaconda に入っている python のモジュールを管理するツール。

- `conda info -e` : デフォルトの python の環境を表示
- `conda list` : インストールされているモジュール一覧
- `conda search <module>` : 利用できるモジュールのバージョン情報
- `conda install <module>` : モジュールのインストール
- `conda update conda` : conda コマンドの更新


## 各種モジュールのインストール

- OpenCV2
```
$ conda install -c menpo opencv
```
- OpenCV3
```
$ conda install -c menpo opencv3
```
- Chainer
```
$ conda install chainer        # <= chainer
$ conda install chainercv      # <= chainer (Chainerによる画像処理モジュール)
```

## とりあえず使う

pythonを起動して使う方法はいろいろありますが。。。
- [Spyder (Python用IDEの一つ)](https://pythonhosted.org/spyder/)を使う
```
$ spyder &
```
- Spyderのコンソールを使う
```
$ jupyter qtconsole &
```
- [jupyter notebook](http://jupyter.org/)で使う
```
$ jupyter notebook
```
jupyter notebookの解説は[こちらのページ](../jupyter)にも少しあります。
