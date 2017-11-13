---
title: リポジトリ登録
---

自分の作ったソースコードをリポジトリ登録して，Git/GitHubで管理する方法

## 1. プロジェクトをローカルリポジトリに登録

管理したいソースプログラム群があるディレクトリに移動して，バージョン管理のための初期化をする。
```
$ cd <target directory>
$ git init
```

これで，バージョン情報を格納する .git/ が作られる。

## 2. バージョン管理するファイル/ディレクトリを登録

バージョン管理したいファイル/ディレクトリ名を指定する。

```
$ git add .            # 現在のディレクトリにある全てのファイル/ディレクトリを登録
$ git add figures/     # ディレクトリ figures/ 以下のファイルを登録
$ git add *.tex        # すべての .tex ファイルを登録
```

## 3. ファイルの内容を初期登録

```
$ git commit -m "はじめてのgit"`
```

`-m`は1行コメントをつけるオプション。初期化のときに限らず，新く作ったファイルを登録するときには，`git add`と`git commit`を随時実行。ここまでは**ローカルリポジトリ**(ローカルマシン上のリポジトリ)のみの設定。

## 4. Gitサーバに登録する

インターネット上のどこからでも最新ファイルを入手できるようにするには，Gitサーバにリポジトリを登録する。

1. git サーバ上に新規リポジトリを作る(GitHubやBitbucketの各webページ上で作る)
2. ローカルリポジトリ(要はgit管理したいプログラム群のあるディレクトリ)をgitサーバ(公開リポジトリ)に登録する。以下はGitHubに登録する例
	```
	$ cd <directory> <= 登録したいファイル群のあるディレクトリ
	$ git remote add origin https://github.com/someone/someprepo.git
	```
	これで，サーバにファイル置き場(リポジトリ)が作られる。origin と記載している部分は，このリポジトリ(`git@github….git`)に対する短縮名。オリジナルなソースコードを登録するときには origin とする事が多いが，別に他の名前にしても良い。`someone/somerepo`の部分は，GitHub上に作ったリポジトリの名前に従って設定する。

	非公開リポジトリ(private repository)に登録する場合は，以下のようにGitHubの認証情報を加える。
	```
	$ git remote add origin https://<username>:<password>@github.com/someone/somerepo.git
	```
	ただ，パスワード情報等を平文打ちして保存するのはセキュリティ上よろしくはないので，ssh keyをGitHubに登録しておいて，ssh通信にするほうが無難。
	```
	$ git remote add origin git@github.com:someone/somerepo.git
	```

3. 登録情報を確認

	```
	$ git remote -v
	```
	登録情報を間違えていたら，以下のコマンドで一旦削除して再登録する

	```
	$ git remote rm origin
	```

4. リモートリポジトリにファイルをアップロード

	```
	$ git push -u origin master  
	```


## 使う!

以下のgitコマンドは，ローカルリポジトリ(手元のディレクトリ)内のみの変更。サーバへの更新やサーバからのダウンロードのため，毎日の更新前には`git pull`, 更新後には`git pull`を使うこと。

- ファイルの変更情報をgitに登録

	```
	$ git commit -a -m "ファイル変更"`
	```
	オプション
	- `-a` 変更されたファイルすべてを対象とする(`git add -u`と同じ)
	- `-m`はコメント(メッセージ)をつける。省略するとviが起動して，長いコメントを入れられる。

- 履歴をみる  

	```
	$ git log  
	commit 989d476c5ab7fb30bb0eb1ca8f5b917860c9c719`
	Author: jun nishii
	Date:   Wed May 24 13:22:45 2017 +0900  
	First commit  
	```

- 最新ファイル(最後にcommitしたもの)と，あるバージョンの比較をしたいとき

	```
	$ git show 989d476c5ab7fb30bb0eb1ca8f5b917860c9c719 --word-diff=color  
	```

-  あるバージョンに戻したい時

	```
	$ git checkout 989d476c5ab7fb30bb0eb1ca8f5b917860c9c719 *  
	```

	ただし，戻したバージョンを最終バージョンにしたいときには，ここでcommitする。(さらにファイルを修正後でももちろんOK)

	```
	$ git commit -a -m "バージョンを戻す"
	```

- あるバージョンでアブストラクトを書いて，次のバージョンで序論を修正，さらに次のバージョンで図を加えたとする。その後，序論の修正に後悔して，序論のみをもとに戻したくなったとき。(特定の修正を取り消したい時)

	```
	$ git revert <序論を修正したバージョンのID>
	```

- commitした後に，少し修正して，さっきのcommitとまとめてしまいたいとき

	```
	$ git commit --amend
	```

- 現在のディレクトリにファイルにcommitしていないのがあるかを確認する

	```
	$ git status
	```

	このときgitコマンドでは無視したいファイルが有る時(LaTeXの一時ファイル等)は，`.gitignore`という名前のファイルをローカルリポジトリ内に作っておく。

	```
	$ cat .gitignore
	*.aux
	*.idx
	*.log
	*.toc
	*.ist
	*.bbl
	*.blg
	*.dvi
	*.ilg
	*.ind
	*.lot
	*.out
	*.synctex.gz
	```

- gitに登録したファイルやディレクトリを消したい時

	```
	$ git rm <file>   or git rm -r <directory>
	```

- gitに登録したファイルやディレクトリの名前を変える

	```
	$ git mv <old name> <new name>
	```
