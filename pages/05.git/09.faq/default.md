---
title: トラブル対応
---

## Q. origin に設定しているリモートリポジトリURLを変更したい。

originに設定しているリポジトリの接続先をhttpsにしていたが，やっぱりssh接続にした
いとか，リモートリポジトリのURLが変更になったときには，以下のようにoriginの設定を変更する。
```
$ git remote set-url origin <new repository>
```

## Q. remote: Repository not found
`git clone`をしたら`remote: Repository not found` と言われた。

A. private repository (非公開のリポジトリ)からgit cloneしようとするとこのエラーが出る。

もしくは，sshで`git clone`する。
```
$ git clone git@github.com/someone/somerepo.git

```
もしくは，(セキュリティ上あまり勧めないが)，以下のように認証情報を加えてgit cloneをする。
```
$ git clone https://<username>:<password>@github.com/someone/somerepo.git
```

## Q. fatal: unable to access
`git push` で`fatal: unable to access 'https://github.com/someone/somerepo.git': The requested URL returned error: 403`と言われた。

A. remote repository の URLにユーザ名を加えたら治った。
```
$ git remote set-url origin https://<user name>@github.com/someone/somerepo.git
```

## Q. fatal: refusing to merge unrelated histories
`git pull`をしたら
````
* branch            master     -\> FETCH_HEAD
	fatal: refusing to merge unrelated histories
````
と言われた。

A. 以下を試す。
````
$ git merge --allow-unrelated-histories origin/master
````
