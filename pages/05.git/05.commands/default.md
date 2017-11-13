---
title: いろいろなコマンド
---

毎日の作業前には最新情報のダウンロードのための`git pull`, 更新後にはサーバへのアップグレード(リモートリポジトリへの登録)のために`git pull`を忘れずにすること。以下はコマンド例
```
$ git pull origin master # サーバoriginのmasterブランチから最新情報取得
$ git push origin master # サーバoriginのmasterブランチに更新情報反映
```

## ローカルリポジトリの操作

以下のgitコマンドは，ローカルリポジトリ(手元のディレクトリ)内のみの変更をする。
変更後には上記の`git push`を忘れずに。

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
	Author: Jun Nishii
	Date:   Wed May 24 13:22:45 2017 +0900  
	First commit  
	```

- `git -add`の取り消し(stagingの取り消し)
	```
	$ git reset HEAD <file name>
	```
	staging状態にあるファイル全てについて取り消す時にはファル名の指定は無しで。
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
	$ git revert \<序論を修正したバージョンのID\>
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
	$ git rm \<file\>   or git rm -r \<directory\>
	```

- gitに登録したファイルやディレクトリの名前を変える

	```
	$ git mv \<old name\> \<new name\>
	```
