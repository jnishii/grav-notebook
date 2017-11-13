---
title: Homebrew
slug: brew
---

以前はMacのパッケージ管理に[MacPorts](../macports)を使っていたけど，[homebrew](http://brew.sh/)の方がインストールサイズが小さくてすむので，こちらに乗り換えた。
以下はインストールメモ。

## HomeBrewのインストール方法

1. **MacPortsから移行する時**: MacPortsから移行する場合はMacPortsをアンインストールしておく。方法は[こちら](../macports)。


2. Xcode のインストール: Xcodeのコマンドラインもインストールしておく。

3. brewコマンドのインストール
```
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
$ brew doctor
```
`brew doctor`の実行後にエラーが出たら，問題を解決しておく。解決方法は大抵エラーメッセージに書いてある。

4. `brew cask` 実行時のインストールディレクトリの設定

	- homebrew-cask (後述)でインストールしたアプリはデフォルトでは`~/Application`にインストールされる。以下の1, 2行目はこれを`/Application`に変更する設定.

	```
	$ echo "export HOMEBREW_CASK_OPTS=\"--appdir=/Applications\"" >> ~/.bash_profile
	```


## 新規にいろいろインストール

適当に必要なソフトを見繕ってインストール。
```
$ brew install --japanese --cocoa --with-gnutls -v emacs
$ brew install lv gnupg nkf rmtrash
$ brew install pandoc markdown
```
- [emacs](http://emacsformacosx.com) (Gnu Emacs)
- lv:  多言語対応のlessみたいなの
- nkf: 文字コード変換ツール
- rmtrash: rmしたものを完全に消さないで，ゴミ箱に移動してくれる
- [pandoc](http://sky-y.github.io/site-pandoc-jp/users-guide/): markdownとかlatexとかdocxとかを相互に変換するツール


## homebrew-caskを使えるようにする

dmgパッケージとして配布されているものも，homebrew-caskを使うとbrewでインストール・管理できるものが多い。
homebrew-caskでインストールすると`brew update`でまとめて更新できるようになるので便利。

### 準備
```
$ brew tap caskroom/cask
```

### いろいろインストール

- [Atom](https://atom.io/): エディタ
- sshfs: ssh経由でファイルマウントするコマンド
- grace: グラフプロッタ
- php70: macのデフォルトのphp以外を使う時

```
$ brew cask install xquartz     # R, grace, その他いくつかのアプリで必要
$ brew cask install atom sshfs
$ brew cask install google-japanese-ime	# google日本語入力
$ brew tap homebrew/x11 # graceのインストールのため
$ brew install grace
$ brew tap homebrew/dupes # phpで必要なzlibインストールのため
$ brew install php70 --with-apache
```

### Rのインストール

R は `homebrew/science`あるものと，`homebrew/cask`にあるものがある。
後者をインストールすると，`/usr/local/lib/`以下にtcl/tk 関連のライブラリがインストールされ，これを消去するようにと`homebrew doctor`がエラーを出す。実際に消すと，Rの実行に困る場合がある。
前者をインストールするとこの問題は無くなるが，gccやら何やら，わさわさとインストールしないといけなくなる(`mpfr, libmpc, isl, gcc, gettext, pcre, pixman, libffi, glib, cairo`がインストールされた...)。ここでは前者の方でインストールする。

```
$ brew tap homebrew/science # r はここにある
$ brew install r   # R言語
$ brew cask install rstudio   # RStudio
$ brew cask install qlmarkdown  # .md のQuickLookプレビュー
```

### Sublime Text 3のインストール

```
$ brew tap caskroom/homebrew-versions # for Sublime Text 3
$ brew cask install sublime-text-dev
```

### gnuplotのインストール

gnuplot のインストールは brew cask で xquartz をインストールした後にする。
```
$ brew cask install aquaterm
$ brew install gnuplot --with-x11 --with-aquaterm
```

### mackup

[mackup](https://github.com/lra/mackup/)は各種設定ファイルをDropbox経由で同期する設定をするツール。
```
$ brew install mackup
$ mackup backup  <= macup新規導入時
$ mackup restore <= macupを他のマシンで利用開始時(既にあるmackupファイルを利用)
```
[同期したいものを追加したい時](https://github.com/lra/mackup/tree/master/doc#add-support-for-an-application-or-any-file-or-directory)には`~/.mackup/<name>.cfg`を追加。

### LaTeXのインストールと設定

1. インストール
```
$ brew cask install mactex
```
パスの設定(terminal上で実行かつ，`~/.bash_profile` 等で設定)
```
$ export PATH=/usr/local/texlive/2016/bin/x86_64-darwin/:$PATH
```
TeXまわりのアップデート
```
$ sudo tlmgr update --self --all
```
2. 設定(Sierra/El Capitanの場合)
```
$ cd /usr/local/texlive/2016/texmf-dist/scripts/cjk-gs-integrate
$ sudo perl cjk-gs-integrate.pl --link-texmf --force
$ sudo mktexlsr
$ sudo updmap-sys --setoption kanjiEmbed hiragino-elcapitan-pron  <= ヒラギノのProN/StdNを使う
```
Nシリーズでないヒラギノ(Pro/Std)を使うこともできるが，[ヒラギノフォントの販売元](http://www.screen.co.jp/ga_product/sento/support/otf_osx_El_Capitan.html)によるとEl Capitan搭載のヒラギノフォント(Pro/Std)には互換性のための深刻な問題があるので使用はしないようにとのこと。

2. 設定(Yosemiteの場合)
```
$ sudo tlmgr install collection-langjapanese collection-latexrecommended collection-fontsrecommended
```
以下の sethiraginofont.sh を実行してヒラギノフォントを使えるようにする。
```
$ cat sethiraginofont.sh
#!/bin/sh
mkdir -p /usr/local/texlive/texmf-local/fonts/opentype/hiragino/
cd /usr/local/texlive/texmf-local/fonts/opentype/hiragino/
ln -fs "/Library/Fonts/ヒラギノ明朝 Pro W3.otf" ./HiraMinPro-W3.otf
ln -fs "/Library/Fonts/ヒラギノ明朝 Pro W6.otf" ./HiraMinPro-W6.otf
ln -fs "/Library/Fonts/ヒラギノ丸ゴ Pro W4.otf" ./HiraMaruPro-W4.otf
ln -fs "/Library/Fonts/ヒラギノ角ゴ Pro W3.otf" ./HiraKakuPro-W3.otf
ln -fs "/Library/Fonts/ヒラギノ角ゴ Pro W6.otf" ./HiraKakuPro-W6.otf
ln -fs "/Library/Fonts/ヒラギノ角ゴ Std W8.otf" ./HiraKakuStd-W8.otf
mktexlsr
updmap-sys --setoption kanjiEmbed hiragino
$ ./sethiraginofont.sh
```


### 参考リンク

- [brew cask で異なるバージョンをインストールする、もしくはアプリを追加する](http://www.d-wood.com/blog/2014/03/27_5895.html)
- [homebrew-cask](http://qiita.com/ryurock/items/1432578d364985f6cb06)


## homebrewのコマンド

- homebrewの更新: `brew update`
- formulaの更新: `brew upgrade`
- パッケージ(formula)削除: `brew uninstall`
- 古いバージョンのformula一覧表示: `brew outdated`
- 古いバージョンのformulaを削除: `brew cleanup`
- インストールされたパッケージ(formula)の一覧表示: `brew list`
- formulaの情報表示: `brew info <formula>`
- formulaの有効化/無効化: `brew link/unlink <formula>`
- brewの健康診断(なにか問題が起きてないかチェック): `brew doctor`
- パッケージ作成
	`brew create http://foo.com/bar-1.0.tgz`
	-  パッケージのmake scriptの修正
	`brew edit bar`


## その他いろいろ

- 他のマシンと同じパッケージをまとめてインストールする方法

以下を実行すると，インストールされているファイル一覧(Brewfile)ができる。
```
$ brew bundle dump
```
これを新規マシンに持って行って以下を実行すると同じパッケージをインストールできる。
```
$ brew bundle
```



## 困ったときのメモ

- "You are in 'detached HEAD' state. "と怒られた時は以下を実行する。
```
$ brew untap phinze/homebrew-cask
$ brew tap caskroom/homebrew-cask
```
- DisplayLinkがインストールする`/usr/local/lib/libusb-…` は brew doctorで怒られるので，削除して `brew install libusb`

### 参考リンク

- [Formula-Cookbook](https://github.com/Homebrew/homebrew/wiki/Formula-Cookbook)
- [homebrewをフォークするためのGit&GitHub入門](http://toggtc.hatenablog.com/entry/2012/02/25/232434)
