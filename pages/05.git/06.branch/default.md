---
title: ブランチ
---

リポジトリをダウンロード(`git clone`)し，一時的に自分なりのカスタマイズをしたり，開発用のテスト版を作りたいときには，派生バージョン(branch)を作って管理できる。

## 派生バージョン(branch)の作成

1. `git clone`でリポジトリのダウンロード
2. ブランチを作る。例えば，branch名を test にする場合は以下の通り。
	```
	$ git checkout -b test
	````

## どのbranchを編集するか変更

リポジトリ内で，複数のbranchの編集が可能。その切替は以下のコマンドで
1. 現在，どのbranchを編集可能か確認
	```
	$ git branch
	```
	サーバ上のブランチもすべて見たいときには`-a`をつける
2. test branchを編集したいときには
	```
	$ git checkout test
	```
3. 作業branchが変わったことを確認
	```
	$ git branch
	```

## branchを，もとのバージョンに反映したい時

作ったtestブランチを，サーバ上にも反映する。
```
$ git push -u origin test
```
`-u`は新規ブランチを始めてサーバにアップロードする時につける。2回目以降は省略可